---
title: Publication Management Flow
---
This document will explore the functionality and flow of the `PublicationController` in the ai-dial-core project, specifically focusing on how it manages publications. We'll cover:

1. The initiation of publication management through `PublicationController`.
2. The deletion of publications and associated resources.
3. The approval process for publications including resource checks and copying.
4. Handling publication listing and resource fetching.

```mermaid
graph TD;
  PublicationController:::mainFlowStyle --> deletePublication
  PublicationController:::mainFlowStyle --> listPublications
  PublicationController:::mainFlowStyle --> listPublishedResources
  PublicationController:::mainFlowStyle --> rejectPublication
  PublicationController:::mainFlowStyle --> approvePublication
  PublicationController:::mainFlowStyle --> iuapv[...]
  deletePublication --> getResource
  deletePublication --> deleteReviewResources
  deletePublication --> deletePublicResources
  deletePublication --> preparePublicationForDeletion
  deletePublication --> ts5su[...]
  deleteReviewResources --> deleteResource
  deleteReviewResources --> 27kw1[...]
  deletePublicResources --> deleteResource
  deletePublicResources --> 1ihc9[...]
  preparePublicationForDeletion --> computeResource
  preparePublicationForDeletion --> z7bo2[...]
  listPublications --> getResource
  listPublications --> wm7ue[...]
  listPublishedResources --> getResource
  listPublishedResources --> 4c9gd[...]
  rejectPublication --> deleteReviewResources
  rejectPublication --> 0b46k[...]
  approvePublication:::mainFlowStyle --> deleteReviewResources
  approvePublication:::mainFlowStyle --> copyReviewToTargetResources
  approvePublication:::mainFlowStyle --> checkReviewResources
  approvePublication:::mainFlowStyle --> checkTargetResources
  approvePublication:::mainFlowStyle --> getPublication
  approvePublication:::mainFlowStyle --> 6k8bq[...]
  deleteResource --> revokeSharedAccess
  deleteResource --> cleanUpResourceLinks
  deleteResource --> 0jk01[...]
  copyReviewToTargetResources --> fromPrivateUrl
  copyReviewToTargetResources --> fromPublicUrl
  copyReviewToTargetResources --> xirhd[...]
  fromPrivateUrl --> fromUrl
  fromPublicUrl --> fromUrl
  checkReviewResources --> checkResource
  checkReviewResources --> fromPrivateUrl
  checkReviewResources --> qi4m6[...]
  checkResource --> hasResource
  checkResource --> fh2mi[...]
  checkTargetResources --> checkResource
  checkTargetResources --> fromPublicUrl
  checkTargetResources --> hwoom[...]
  getPublication:::mainFlowStyle --> getResource
  getPublication:::mainFlowStyle --> respondError
  getPublication:::mainFlowStyle --> phnhd[...]
  respondError:::mainFlowStyle --> respond
  respond:::mainFlowStyle --> end
  respond:::mainFlowStyle --> h7gu9[...]
  end:::mainFlowStyle --> handle
  end:::mainFlowStyle --> gk06l[...]
  handle:::mainFlowStyle --> getResource
  handle:::mainFlowStyle --> handleRateLimitSuccess
  handle:::mainFlowStyle --> deleteResource
  handle:::mainFlowStyle --> limit
  handle:::mainFlowStyle --> handleRequest
  handle:::mainFlowStyle --> 38jky[...]
  handleRateLimitSuccess --> handleRequestBody
  handleRateLimitSuccess --> initFromContext
  handleRateLimitSuccess --> getDeployment
  handleRateLimitSuccess --> isrh4[...]
  handleRequestBody --> enhanceAssistantRequest
  handleRequestBody --> sendRequest
  handleRequestBody --> enhanceModelRequest
  handleRequestBody --> getDeployment
  handleRequestBody --> u0axo[...]
  initFromContext --> getDeployment
  initFromContext --> jpdvk[...]
  getDeployment --> getModels
  getDeployment --> 4lsum[...]
  revokeSharedAccess --> getResource
  revokeSharedAccess --> removeSharedResource
  revokeSharedAccess --> computeResource
  revokeSharedAccess --> getResourceFromLink
  revokeSharedAccess --> dkywk[...]
  cleanUpResourceLinks --> getInvitations
  cleanUpResourceLinks --> computeResource
  cleanUpResourceLinks --> a2gq1[...]
  limit --> checkLimit
  limit --> getDeployment
  limit --> 44t48[...]
  checkLimit --> checkRequestLimit
  checkLimit --> checkTokenLimit
  handleRequest:::mainFlowStyle --> onExtractClaimsSuccess
  handleRequest:::mainFlowStyle --> nl22b[...]
  onExtractClaimsSuccess:::mainFlowStyle --> select
  onExtractClaimsSuccess:::mainFlowStyle --> dvlje[...]
  select:::mainFlowStyle --> ...

 classDef mainFlowStyle color:#000000,fill:#7CB9F4
```

<SwmSnippet path="/src/main/java/com/epam/aidial/core/controller/PublicationController.java" line="91">

---

# Initiation of Publication Management

The `PublicationController` starts managing publications by handling various publication-related requests such as delete, list, and approve. This is the entry point for publication management.

```java
    public Future<?> deletePublication() {
        context.getRequest()
                .body()
                .compose(body -> {
                    String url = ProxyUtil.convertToObject(body, ResourceLink.class).url();
                    ResourceDescription resource = decodePublication(url, false);
                    checkAccess(resource, true);
                    return vertx.executeBlocking(() ->
                            lockService.underBucketLock(BlobStorageUtil.PUBLIC_LOCATION,
                                    () -> publicationService.deletePublication(resource, isAdmin())));
                })
                .onSuccess(publication -> context.respond(HttpStatus.OK))
                .onFailure(error -> respondError("Can't delete publication", error));

        return Future.succeededFuture();
    }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/controller/PublicationController.java" line="91">

---

# Deleting Publications

The `deletePublication` method in `PublicationController` orchestrates the deletion of a publication. It involves checking access permissions, locking the resource, and finally deleting the publication and its associated resources.

```java
    public Future<?> deletePublication() {
        context.getRequest()
                .body()
                .compose(body -> {
                    String url = ProxyUtil.convertToObject(body, ResourceLink.class).url();
                    ResourceDescription resource = decodePublication(url, false);
                    checkAccess(resource, true);
                    return vertx.executeBlocking(() ->
                            lockService.underBucketLock(BlobStorageUtil.PUBLIC_LOCATION,
                                    () -> publicationService.deletePublication(resource, isAdmin())));
                })
                .onSuccess(publication -> context.respond(HttpStatus.OK))
                .onFailure(error -> respondError("Can't delete publication", error));

        return Future.succeededFuture();
    }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/controller/PublicationController.java" line="91">

---

# Approving Publications

The `approvePublication` method handles the approval of publications. It includes operations like deleting review resources, copying resources from review to target, and checking both review and target resources to ensure they meet certain criteria before final approval.

```java
    public Future<?> deletePublication() {
        context.getRequest()
                .body()
                .compose(body -> {
                    String url = ProxyUtil.convertToObject(body, ResourceLink.class).url();
                    ResourceDescription resource = decodePublication(url, false);
                    checkAccess(resource, true);
                    return vertx.executeBlocking(() ->
                            lockService.underBucketLock(BlobStorageUtil.PUBLIC_LOCATION,
                                    () -> publicationService.deletePublication(resource, isAdmin())));
                })
                .onSuccess(publication -> context.respond(HttpStatus.OK))
                .onFailure(error -> respondError("Can't delete publication", error));

        return Future.succeededFuture();
    }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/controller/PublicationController.java" line="91">

---

# Listing Publications and Resources

Methods like `listPublications` and `listPublishedResources` in `PublicationController` are responsible for fetching lists of publications and their associated resources. These methods ensure that resources are fetched correctly and errors are handled appropriately.

```java
    public Future<?> deletePublication() {
        context.getRequest()
                .body()
                .compose(body -> {
                    String url = ProxyUtil.convertToObject(body, ResourceLink.class).url();
                    ResourceDescription resource = decodePublication(url, false);
                    checkAccess(resource, true);
                    return vertx.executeBlocking(() ->
                            lockService.underBucketLock(BlobStorageUtil.PUBLIC_LOCATION,
                                    () -> publicationService.deletePublication(resource, isAdmin())));
                })
                .onSuccess(publication -> context.respond(HttpStatus.OK))
                .onFailure(error -> respondError("Can't delete publication", error));

        return Future.succeededFuture();
    }
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYWktZGlhbC1jb3JlJTNBJTNBZXBhbQ==" repo-name="ai-dial-core"><sup>Powered by [Swimm](/)</sup></SwmMeta>
