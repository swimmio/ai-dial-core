---
title: Publication Rejection Process
---
This document will cover the process of rejecting a publication in the ai-dial-core project. The process includes the following steps:

1. Invoking the PublicationController
2. Calling the rejectPublication method
3. Checking access with the checkAccess method
4. Verifying admin status with the isAdmin method
5. Checking admin access with the hasAdminAccess method.

<SwmSnippet path="/src/main/java/com/epam/aidial/core/controller/PublicationController.java" line="34">

---

# Invoking the PublicationController

The `PublicationController` is the entry point for this flow. It is initialized with various services including `PublicationService`, `AccessService`, and `EncryptionService`.

```java
    private final PublicationService publicationService;
    private final ProxyContext context;

    public PublicationController(Proxy proxy, ProxyContext context) {
        this.vertx = proxy.getVertx();
        this.lockService = proxy.getLockService();
        this.accessService = proxy.getAccessService();
        this.encryptService = proxy.getEncryptionService();
        this.publicationService = proxy.getPublicationService();
        this.context = context;
    }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/controller/PublicationController.java" line="125">

---

# Calling the rejectPublication method

The `rejectPublication` method is called next. This method handles the rejection of a publication. It first decodes the publication and then checks access before rejecting the publication.

```java
    public Future<?> rejectPublication() {
        context.getRequest()
                .body()
                .compose(body -> {
                    String url = ProxyUtil.convertToObject(body, ResourceLink.class).url();
                    ResourceDescription resource = decodePublication(url, false);
                    checkAccess(resource, false);
                    return vertx.executeBlocking(() -> publicationService.rejectPublication(resource));
                })
                .onSuccess(publication -> context.respond(HttpStatus.OK, publication))
                .onFailure(error -> respondError("Can't reject publication", error));

        return Future.succeededFuture();
    }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/controller/PublicationController.java" line="228">

---

# Checking access with the checkAccess method

The `checkAccess` method is used to verify if the user has the necessary permissions to reject the publication. It checks if the user is an admin and if the resource belongs to the user.

```java
    private void checkAccess(ResourceDescription resource, boolean allowUser) {
        boolean hasAccess = isAdmin();

        if (!hasAccess && allowUser) {
            String bucket = BlobStorageUtil.buildInitiatorBucket(context);
            hasAccess = resource.getBucketLocation().equals(bucket);
        }

        if (!hasAccess) {
            throw new HttpException(HttpStatus.FORBIDDEN, "Forbidden resource: " + resource.getUrl());
        }
    }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/controller/PublicationController.java" line="241">

---

# Verifying admin status with the isAdmin method

The `isAdmin` method is used to check if the user has admin privileges. It does this by calling the `hasAdminAccess` method of the `AccessService`.

```java
    private boolean isAdmin() {
        return accessService.hasAdminAccess(context);
    }
```

---

</SwmSnippet>

# Checking admin access with the hasAdminAccess method

The `hasAdminAccess` method is the final step in the flow. It checks if the user has admin access. Unfortunately, the code for this method is not provided in the context.

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*


