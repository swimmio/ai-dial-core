---
title: Storage Implementations and Usage
---
This document will cover the implementation and functionality of Storage in the ai-dial-core project, specifically focusing on:

# Overview of Storage

Storage in the ai-dial-core project refers to the mechanisms and structures used for storing and managing data across various storage providers like AWS S3, Google Cloud Storage, and Azure Blob Storage. It is implemented in various classes within the `com.epam.aidial.core.storage` package.

<SwmSnippet path="/src/main/java/com/epam/aidial/core/storage/StorageProvider.java" line="1">

---

# Storage Implementations

The `StorageProvider` enum defines the types of storage available, including S3, AWS_S3, FILESYSTEM, GOOGLE_CLOUD_STORAGE, and AZURE_BLOB. This allows the system to support multiple storage backends.

```java
package com.epam.aidial.core.storage;

public enum StorageProvider {
    S3, AWS_S3, FILESYSTEM, GOOGLE_CLOUD_STORAGE, AZURE_BLOB;

    public static StorageProvider from(String storageProviderName) {
        return switch (storageProviderName) {
            case "s3" -> S3;
            case "aws-s3" -> AWS_S3;
            case "azureblob" -> AZURE_BLOB;
            case "google-cloud-storage" -> GOOGLE_CLOUD_STORAGE;
            case "filesystem" -> FILESYSTEM;
            default -> throw new IllegalArgumentException("Unknown storage provider");
        };
    }
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/storage/BlobStorage.java" line="59">

---

# Blob Storage Usage

The `BlobStorage` class is a key component in handling storage operations. It initializes and manages connections to the storage provider, handles file uploads, and manages multipart uploads. This class uses the storage provider settings to perform operations like storing and retrieving blobs.

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

    /**
     * Initialize multipart upload
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYWktZGlhbC1jb3JlJTNBJTNBZXBhbQ==" repo-name="ai-dial-core"><sup>Powered by [Swimm](/)</sup></SwmMeta>
