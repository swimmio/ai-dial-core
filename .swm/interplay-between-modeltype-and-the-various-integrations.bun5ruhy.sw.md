---
title: Interplay between ModelType and the various Integrations
---
This document will cover the interaction of 'ModelType' with various integrations like Google Cloud Storage, Azure Blob Store, and Redis. We'll cover:

1. How 'ModelType' is defined and used in the codebase
2. How 'ModelType' interacts with Google Cloud Storage, Azure Blob Store, and Redis

<SwmSnippet path="/src/main/java/com/epam/aidial/core/storage/BlobStorage.java" line="5">

---

# 'ModelType' in the codebase

The 'ModelType' is not directly defined in the codebase. However, the 'BlobStorage' class imports various packages that handle different types of data, which could be considered as 'ModelType'.

```java
import com.epam.aidial.core.data.MetadataBase;
import com.epam.aidial.core.data.ResourceFolderMetadata;
import com.epam.aidial.core.data.ResourceType;
import com.epam.aidial.core.storage.credential.CredentialProvider;
import com.epam.aidial.core.storage.credential.CredentialProviderFactory;
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/storage/BlobStorage.java" line="202">

---

# Interaction with Google Cloud Storage, Azure Blob Store, and Redis

The 'BlobStorage' class has methods like 'listMetadata' and 'buildFileMetadata' that interact with the storage. These methods use 'ResourceDescription', which could be a representation of 'ModelType', to interact with the storage. However, the specific interaction with Google Cloud Storage, Azure Blob Store, and Redis is not directly visible in the provided context.

```java
     */
    public MetadataBase listMetadata(ResourceDescription resource) {
        String storageLocation = getStorageLocation(resource.getAbsoluteFilePath());
        ListContainerOptions options = buildListContainerOptions(storageLocation);
        PageSet<? extends StorageMetadata> list = blobStore.list(this.bucketName, options);
        List<MetadataBase> filesMetadata = list.stream().map(meta -> buildFileMetadata(resource, meta)).toList();

        // listing folder
        if (resource.isFolder()) {
            boolean isEmpty = filesMetadata.isEmpty() && !resource.isRootFolder();
            return isEmpty ? null : new ResourceFolderMetadata(resource, filesMetadata);
        } else {
            // listing file
            if (filesMetadata.size() == 1) {
                return filesMetadata.get(0);
            }
            return null;
        }
    }

    public PageSet<? extends StorageMetadata> list(String absoluteFilePath, String afterMarker, int maxResults, boolean recursive) {
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*


