---
title: Management of Storage Settings and Endpoint Configuration
---
This document will cover the management of storage settings, specifically focusing on endpoint configuration in the ai-dial-core project. We'll explore:

1. How storage settings are defined within the codebase.
2. The use of these settings in managing storage operations.

<SwmSnippet path="/src/main/java/com/epam/aidial/core/config/Storage.java" line="8">

---

# Storage Configuration Overview

The `Storage` class in `Storage.java` is central to managing storage settings. It includes fields for provider, endpoint, identity, credential, bucket, createBucket, overrides, and prefix. These settings configure the storage provider and manage how data is stored and accessed.

```java
@Data
public class Storage {
    /**
     * Specifies storage provider. Supported providers: s3, aws-s3, azureblob, google-cloud-storage, filesystem
     */
    String provider;
    /**
     * Optional. Specifies endpoint url for s3 compatible storages
     */
    @Nullable
    String endpoint;
    /**
     * Api key. Optional for filesystem and aws-s3 (will try to get token from EC2 instance metadata)
     */
    @Nullable
    String identity;
    /**
     * Secret key. Optional for filesystem and aws-s3 (will try to get token from EC2 instance metadata)
     */
    @Nullable
    String credential;
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/storage/BlobStorage.java" line="59">

---

# Utilizing Storage Configuration

The `BlobStorage` class utilizes the `Storage` configuration for operations such as initializing the storage provider and creating buckets if needed. The constructor and `createBucketIfNeeded` method show how the storage settings are applied to manage data storage effectively.

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

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYWktZGlhbC1jb3JlJTNBJTNBc3dpbW1pbw==" repo-name="ai-dial-core"><sup>Powered by [Swimm](/)</sup></SwmMeta>
