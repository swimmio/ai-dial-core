---
title: Overview of Core Services
---
This document will cover the implementation and functionality of services in the ai-dial-core project, specifically focusing on the `Service` classes within the `src/main/java/com/epam/aidial/core/service` directory. We'll explore:

1. The role and functionality of services.
2. Key implementations of these services.

# Service Functionality and Usage

Services in the ai-dial-core project are designed to handle business logic and operations related to resources, sharing, and invitations. They interact with storage systems, manage synchronization, and handle resource metadata. These services are crucial for the operation of the system as they provide a layer of abstraction over direct data manipulation, ensuring data consistency and integrity.

<SwmSnippet path="/src/main/java/com/epam/aidial/core/service/ResourceService.java" line="36">

---

# Implementation of Services

The `ResourceService` class is a comprehensive example of a service in this project. It manages resource metadata, handles synchronization with Redis and Blob storage, and supports operations like get, put, delete, and sync of resources. This service uses various configurations such as maximum size limits, synchronization periods, and cache settings to manage resources efficiently.

```java
@Slf4j
public class ResourceService implements AutoCloseable {
    private static final Set<String> REDIS_FIELDS = Set.of("body", "created_at", "updated_at", "synced", "exists");
    private static final Set<String> REDIS_FIELDS_NO_BODY = Set.of("created_at", "updated_at", "synced", "exists");

    private static final Result BLOB_NOT_FOUND = new Result("", Long.MIN_VALUE, Long.MIN_VALUE, true, false);

    private final Vertx vertx;
    private final RedissonClient redis;
    private final BlobStorage blobStore;
    private final LockService lockService;
    @Getter
    private final int maxSize;
    private final long syncTimer;
    private final long syncDelay;
    private final int syncBatch;
    private final Duration cacheExpiration;
    private final int compressionMinSize;
    private final String prefix;
    private final String resourceQueue;

```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/service/ShareService.java" line="32">

---

# Implementation of Services

The `ShareService` class handles operations related to resource sharing. It provides functionality to list resources shared with/by the user, initialize share requests, and manage access permissions. This service interacts closely with `ResourceService` and `InvitationService` to manage shared resources and invitations.

```java
@Slf4j
@AllArgsConstructor
public class ShareService {

    private static final String SHARE_RESOURCE_FILENAME = "share";

    private final ResourceService resourceService;
    private final InvitationService invitationService;
    private final EncryptionService encryptionService;
    private final BlobStorage storage;

    /**
     * Returns a list of resources shared with user.
     *
     * @param bucket - user bucket
     * @param location - storage location
     * @param request - request body
     * @return list of shared with user resources
     */
    public SharedResourcesResponse listSharedWithMe(String bucket, String location, ListSharedResourcesRequest request) {
        Set<ResourceType> requestedResourceType = request.getResourceTypes();
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYWktZGlhbC1jb3JlJTNBJTNBZXBhbQ==" repo-name="ai-dial-core"><sup>Powered by [Swimm](/)</sup></SwmMeta>
