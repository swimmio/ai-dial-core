---
title: Publication Request Handling Flow
---
This document will cover the flow of handling a publication request in the ai-dial-core project. We'll cover:

1. The initiation of the flow by the PublicationController
2. The approval of the publication
3. The retrieval of the publication
4. The handling of errors and responses
5. The handling of the request and extraction of claims.

<SwmSnippet path="/src/main/java/com/epam/aidial/core/controller/PublicationController.java" line="34">

---

# Initiation by the PublicationController

The `PublicationController` is the entry point of the flow. It is initialized with a `Proxy` and a `ProxyContext`. The `Proxy` provides services like `LockService`, `AccessService`, `EncryptionService`, and `PublicationService` which are used in the flow.

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

<SwmSnippet path="/src/main/java/com/epam/aidial/core/service/PublicationService.java" line="267">

---

# Approval of the Publication

`approvePublication` is called by the `PublicationController`. This method checks the status of the publication, verifies the review and target resources, and updates the status of the publication to `APPROVED`.

```java
    @Nullable
    public Publication approvePublication(ResourceDescription resource) {
        Publication publication = getPublication(resource);
        if (publication.getStatus() != Publication.Status.PENDING) {
            throw new ResourceNotFoundException("Publication is already finalized: " + resource.getUrl());
        }

        checkReviewResources(publication);
        checkTargetResources(publication);

        resources.computeResource(publications(resource), body -> {
            Map<String, Publication> publications = decodePublications(body);
            Publication previous = publications.put(resource.getUrl(), publication);

            if (!publication.equals(previous)) {
                throw new ResourceNotFoundException("Publication changed during approving: " + resource.getUrl());
            }

            publication.setStatus(Publication.Status.APPROVED);
            return encodePublications(publications);
        });
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/controller/PublicationController.java" line="61">

---

# Retrieval of the Publication

`getPublication` is called within `approvePublication`. It retrieves the publication and responds with the publication if successful, or calls `respondError` if there's an error.

```java
    public Future<?> getPublication() {
        context.getRequest()
                .body()
                .compose(body -> {
                    String url = ProxyUtil.convertToObject(body, ResourceLink.class).url();
                    ResourceDescription resource = decodePublication(url, false);
                    checkAccess(resource, true);
                    return vertx.executeBlocking(() -> publicationService.getPublication(resource));
                })
                .onSuccess(publication -> context.respond(HttpStatus.OK, publication))
                .onFailure(error -> respondError("Can't get publication", error));

        return Future.succeededFuture();
    }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/controller/PublicationController.java" line="170">

---

# Handling Errors and Responses

`respondError` is used to handle errors during the flow. It determines the HTTP status based on the type of error and calls `respond` to send the response.

```java
    private void respondError(String message, Throwable error) {
        HttpStatus status = HttpStatus.INTERNAL_SERVER_ERROR;
        String body = null;

        if (error instanceof HttpException e) {
            status = e.getStatus();
            body = e.getMessage();
        } else if (error instanceof ResourceNotFoundException) {
            status = HttpStatus.NOT_FOUND;
            body = error.getMessage();
        } else if (error instanceof IllegalArgumentException e) {
            status = HttpStatus.BAD_REQUEST;
            body = e.getMessage();
        } else {
            log.warn(message, error);
        }

        context.respond(status, body);
    }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/Proxy.java" line="102">

---

# Handling the Request and Extraction of Claims

`handleRequest` is called by `handle`. It validates the request and extracts claims from the authorization header. If successful, it calls `onExtractClaimsSuccess`.

```java
    /**
     * Called when proxy received the request headers from a client.
     */
    private void handleRequest(HttpServerRequest request) {
        if (request.version() != HttpVersion.HTTP_1_1) {
            respond(request, HttpStatus.HTTP_VERSION_NOT_SUPPORTED);
            return;
        }

        HttpMethod requestMethod = request.method();
        if (!ALLOWED_HTTP_METHODS.contains(requestMethod)) {
            respond(request, HttpStatus.METHOD_NOT_ALLOWED);
            return;
        }

        String contentType = request.getHeader(HttpHeaders.CONTENT_TYPE);
        int contentLength = ProxyUtil.contentLength(request, 1024);
        if (contentType != null && contentType.startsWith("multipart/form-data")) {
            if (contentLength > FILES_REQUEST_BODY_MAX_SIZE_BYTES) {
                respond(request, HttpStatus.REQUEST_ENTITY_TOO_LARGE, "Request body is too large");
                return;
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*


