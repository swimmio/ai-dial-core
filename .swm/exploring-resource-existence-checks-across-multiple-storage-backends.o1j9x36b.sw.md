---
title: Exploring Resource Existence Checks Across Multiple Storage Backends
---
This document will explain how resource existence checks are performed in ai-dial-core when dealing with multiple storage backends. The key points covered include:

1. How resources are described and identified across different storage backends.
2. The process of checking the existence of resources in various storage types.

<SwmSnippet path="/src/main/java/com/epam/aidial/core/storage/ResourceDescription.java" line="161">

---

# Resource Description and Identification

Resource descriptions are crucial for identifying resources across different storage backends. The method `fromPrivateUrl` helps in constructing a `ResourceDescription` from a URL, which is essential for subsequent existence checks.

```java
    /**
     * @param url      must contain the same bucket and location as resource.
     * @param resource to take bucket and location from.
     */
    public static ResourceDescription fromPrivateUrl(String url, ResourceDescription resource) {
        return fromUrl(url, resource.getBucketName(), resource.getBucketLocation(), null);
    }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/util/ResourceUtil.java" line="11">

---

# Checking Resource Existence

The method `hasResource` in `ResourceUtil` plays a central role in checking the existence of resources. It handles different types of resources like files, conversations, and prompts by calling appropriate methods in the storage or resource service.

```java
    public static boolean hasResource(ResourceDescription resource, ResourceService resourceService, BlobStorage storage) {
        return switch (resource.getType()) {
            case FILE -> storage.exists(resource.getAbsoluteFilePath());
            case CONVERSATION, PROMPT -> resourceService.hasResource(resource);
            default -> throw new IllegalArgumentException("Unsupported resource type " + resource.getType());
        };
    }
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYWktZGlhbC1jb3JlJTNBJTNBZXBhbQ==" repo-name="ai-dial-core"><sup>Powered by [Swimm](/)</sup></SwmMeta>
