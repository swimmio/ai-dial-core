---
title: Resource Access Management Flow
---
This document will explain the code flow initiated by the `moveResource` function, detailing each function's role in the process of managing shared resource access:

```mermaid
graph TD;
  moveResource:::mainFlowStyle --> moveSharedAccess
  moveResource:::mainFlowStyle --> fl57q[...]
  moveSharedAccess:::mainFlowStyle --> revokeSharedAccess
  moveSharedAccess:::mainFlowStyle --> copySharedAccess
  moveSharedAccess:::mainFlowStyle --> 07cm0[...]
  revokeSharedAccess --> removeSharedResource
  revokeSharedAccess --> 0pujc[...]
  copySharedAccess:::mainFlowStyle --> addSharedResource
  copySharedAccess:::mainFlowStyle --> bgp8f[...]
  addSharedResource:::mainFlowStyle --> computeResource
  addSharedResource:::mainFlowStyle --> c8uoc[...]
  computeResource:::mainFlowStyle --> lock
  computeResource:::mainFlowStyle --> g68fv[...]
  lock:::mainFlowStyle --> tryLock
  lock:::mainFlowStyle --> ckm1p[...]
  tryLock:::mainFlowStyle --> unlock
  tryLock:::mainFlowStyle --> yvo99[...]
  unlock:::mainFlowStyle --> tryUnlock
  tryUnlock:::mainFlowStyle --> of
  of:::mainFlowStyle --> ...

 classDef mainFlowStyle color:#000000,fill:#7CB9F4
```

<SwmSnippet path="/src/main/java/com/epam/aidial/core/service/ShareService.java" line="357">

---

# Overview of `moveResource` Function

The `moveResource` function orchestrates the process of moving shared access from one resource to another. It first copies the shared access and then revokes the original shared access.

```java
    public void moveSharedAccess(String bucket, String location, ResourceDescription source, ResourceDescription destination) {
        // copy shared access from source to destination
        copySharedAccess(bucket, location, source, destination);
        // revoke shared access from source
        revokeSharedAccess(bucket, location, new ResourceLinkCollection(Set.of(new ResourceLink(source.getUrl()))));
    }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/service/ShareService.java" line="359">

---

# Step 1: Copy Shared Access

The `copySharedAccess` function is called to duplicate the shared access settings from the source to the destination resource. This is the first step in the resource moving process.

```java
        copySharedAccess(bucket, location, source, destination);
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/service/ShareService.java" line="361">

---

# Step 2: Revoke Original Shared Access

After successfully copying the shared access, `revokeSharedAccess` is invoked to remove the shared access from the original resource, ensuring that the access privileges are only valid at the new location.

```java
        revokeSharedAccess(bucket, location, new ResourceLinkCollection(Set.of(new ResourceLink(source.getUrl()))));
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYWktZGlhbC1jb3JlJTNBJTNBZXBhbQ==" repo-name="ai-dial-core"><sup>Powered by [Swimm](/)</sup></SwmMeta>
