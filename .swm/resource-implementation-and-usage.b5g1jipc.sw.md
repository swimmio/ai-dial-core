---
title: Resource Implementation and Usage
---
This document will explore the implementation and usage of Resource in the ai-dial-core project, specifically within the service layer. We'll cover:

1. What Resources do in the system
2. How Resources are used across different services

# Resource Functionality

Resources in the ai-dial-core project are primarily used for managing data persistence and state across various services. They interact with storage systems like Redis and Blob storage to ensure data consistency and availability.

<SwmSnippet path="/src/main/java/com/epam/aidial/core/service/ResourceService.java" line="182">

---

# Implementation Across Services

The `hasResource` method in `ResourceService` checks for the existence of a resource in Redis and Blob storage, utilizing helper methods like `redisKey` and `redisGet` to fetch and verify resource data.

```java
    public boolean hasResource(ResourceDescription descriptor) {
        String redisKey = redisKey(descriptor);
        Result result = redisGet(redisKey, false);

        if (result == null) {
            String blobKey = blobKey(descriptor);
            return blobExists(blobKey);
        }

        return result.exists;
    }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/service/ResourceOperationService.java" line="27">

---

In `ResourceOperationService`, the `moveResource` method uses `hasResource` to ensure the source resource exists before proceeding with operations, demonstrating how resource checks are critical to the service's functionality.

```java
        if (!hasResource(source)) {
            throw new IllegalArgumentException("Source resource %s do not exists".formatted(sourceResourceUrl));
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/service/ShareService.java" line="314">

---

The `copySharedAccess` method in `ShareService` also relies on `hasResource` to validate both source and destination resources, highlighting the method's reuse across different contexts within the service layer.

```java
    public void copySharedAccess(String bucket, String location, ResourceDescription source, ResourceDescription destination) {
        if (!hasResource(source)) {
            throw new IllegalArgumentException("source resource %s does not exists".formatted(source.getUrl()));
        }

        if (!hasResource(destination)) {
            throw new IllegalArgumentException("destination resource %s dos not exists".formatted(destination.getUrl()));
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYWktZGlhbC1jb3JlJTNBJTNBZXBhbQ==" repo-name="ai-dial-core"><sup>Powered by [Swimm](/)</sup></SwmMeta>
