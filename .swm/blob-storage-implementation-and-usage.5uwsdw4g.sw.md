---
title: Blob Storage Implementation and Usage
---
This document will explore Blob Storage in the ai-dial-core project, focusing on its implementation and usage within the codebase:

# Blob Storage Overview

Blob Storage in ai-dial-core is utilized for managing large data objects across various storage services. It abstracts storage implementations through a unified API, allowing for operations like storing, retrieving, and managing data in a scalable manner.

<SwmSnippet path="/src/main/java/com/epam/aidial/core/storage/BlobStorage.java" line="59">

---

# Implementation Details

The `BlobStorage` class is initialized with configuration settings that define the storage provider, credentials, and operational details. This setup is crucial for connecting to the appropriate blob store service.

```java
    public BlobStorage(Storage config) {
        String provider = config.getProvider();
        ContextBuilder builder = ContextBuilder.newBuilder(provider);
        if (config.getEndpoint() != null) {
            builder.endpoint(config.getEndpoint());
        }
        Properties overrides = config.getOverrides();
        if (overrides != null) {
            builder.overrides(overrides);
        }
        CredentialProvider credentialProvider = CredentialProviderFactory.create(provider, config.getIdentity(), config.getCredential());
        builder.credentialsSupplier(credentialProvider::getCredentials);
        this.storeContext = builder.buildView(BlobStoreContext.class);
        this.blobStore = storeContext.getBlobStore();
        this.bucketName = config.getBucket();
        this.prefix = config.getPrefix();
        createBucketIfNeeded(config);
    }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/storage/BlobStorage.java" line="128">

---

# Usage of Blob Storage

This snippet shows how the `store` method is used to upload a file to Blob Storage. It demonstrates setting up the blob's metadata and committing the data to the specified bucket.

```java
    public void store(String absoluteFilePath, String contentType, Buffer data) {
        String storageLocation = getStorageLocation(absoluteFilePath);
        Blob blob = blobStore.blobBuilder(storageLocation)
                .payload(new BufferPayload(data))
                .contentLength(data.length())
                .contentType(contentType)
                .build();

        blobStore.putBlob(bucketName, blob);
    }
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYWktZGlhbC1jb3JlJTNBJTNBZXBhbQ==" repo-name="ai-dial-core"><sup>Powered by [Swimm](/)</sup></SwmMeta>
