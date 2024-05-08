---
title: Transactional Behaviors in Resource Operations
---
This document will explore how resource operations in the ai-dial-core project support transactional behaviors, focusing on the following aspects:

1. How transactional integrity is maintained during resource operations.
2. The role of services and utilities in ensuring transactional behaviors.

<SwmSnippet path="/src/main/java/com/epam/aidial/core/controller/ResourceOperationController.java" line="37">

---

# Transactional Integrity in Resource Operations

The `move` method in `ResourceOperationController` demonstrates transactional behavior by ensuring that all operations either complete successfully or revert changes. It uses `Vertx.executeBlocking` to handle resource locking and calls `resourceOperationService.moveResource` to perform the actual resource movement, ensuring atomicity.

```java
    public Future<?> move() {
        context.getRequest()
                .body()
                .compose(buffer -> {
                    MoveResourcesRequest request;
                    try {
                        request = ProxyUtil.convertToObject(buffer, MoveResourcesRequest.class);
                    } catch (Exception e) {
                        log.error("Invalid request body provided", e);
                        throw new IllegalArgumentException("Can't initiate move resource request. Incorrect body provided");
                    }

                    String sourceUrl = request.getSourceUrl();
                    if (sourceUrl == null) {
                        throw new IllegalArgumentException("sourceUrl must be provided");
                    }

                    String destinationUrl = request.getDestinationUrl();
                    if (destinationUrl == null) {
                        throw new IllegalArgumentException("destinationUrl must be provided");
                    }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/service/ResourceOperationService.java" line="17">

---

# Role of Services and Utilities

The `ResourceOperationService` class plays a crucial role in supporting transactional behaviors by handling the complexities of resource movement. It checks resource existence, manages resource types, and coordinates with other services like `InvitationService` and `ShareService` to update related data, ensuring consistency across the system.

```java
    public void moveResource(String bucket, String location, ResourceDescription source, ResourceDescription destination, boolean overwriteIfExists) {
        if (source.isFolder() || destination.isFolder()) {
            throw new IllegalArgumentException("Moving folders is not supported");
        }

        String sourceResourcePath = source.getAbsoluteFilePath();
        String sourceResourceUrl = source.getUrl();
        String destinationResourcePath = destination.getAbsoluteFilePath();
        String destinationResourceUrl = destination.getUrl();

        if (!hasResource(source)) {
            throw new IllegalArgumentException("Source resource %s do not exists".formatted(sourceResourceUrl));
        }

        ResourceType resourceType = source.getType();
        switch (resourceType) {
            case FILE -> {
                if (!overwriteIfExists && storage.exists(destinationResourcePath)) {
                    throw new IllegalArgumentException("Can't move resource %s to %s, because destination resource already exists"
                            .formatted(sourceResourceUrl, destinationResourceUrl));
                }
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYWktZGlhbC1jb3JlJTNBJTNBc3dpbW1pbw==" repo-name="ai-dial-core"><sup>Powered by [Swimm](/)</sup></SwmMeta>
