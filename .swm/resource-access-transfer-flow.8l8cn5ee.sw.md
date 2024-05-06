---
title: Resource Access Transfer Flow
---
This document will explain the flow initiated by the `moveResource` function, detailing each step and its purpose in the context of resource management in the ai-dial-core project. The steps include:

```mermaid
graph TD;
  moveResource:::mainFlowStyle --> moveSharedAccess
  moveResource:::mainFlowStyle --> tne7w[...]
  moveSharedAccess:::mainFlowStyle --> revokeSharedAccess
  moveSharedAccess:::mainFlowStyle --> copySharedAccess
  moveSharedAccess:::mainFlowStyle --> h9nxy[...]
  revokeSharedAccess --> removeSharedResource
  revokeSharedAccess --> tktqg[...]
  copySharedAccess:::mainFlowStyle --> addSharedResource
  copySharedAccess:::mainFlowStyle --> 95f22[...]
  addSharedResource:::mainFlowStyle --> computeResource
  addSharedResource:::mainFlowStyle --> wd26z[...]
  computeResource:::mainFlowStyle --> lock
  computeResource:::mainFlowStyle --> pfx0n[...]
  lock:::mainFlowStyle --> tryLock
  lock:::mainFlowStyle --> 73e39[...]
  tryLock:::mainFlowStyle --> unlock
  tryLock:::mainFlowStyle --> ok4sh[...]
  unlock:::mainFlowStyle --> tryUnlock
  tryUnlock:::mainFlowStyle --> of
  of:::mainFlowStyle --> ...

 classDef mainFlowStyle color:#000000,fill:#7CB9F4
```

<SwmSnippet path="/src/main/java/com/epam/aidial/core/service/ShareService.java" line="357">

---

# Overview of `moveResource` Function

The `moveResource` function orchestrates the process of transferring shared access of a resource from one location to another. It first copies the shared access and then revokes the original shared access, ensuring the resource is only accessible from the new location.

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

The `copySharedAccess` function is called to replicate the shared access settings from the source to the destination. This ensures that all shared access permissions are maintained during the move.

```java
        copySharedAccess(bucket, location, source, destination);
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/service/ShareService.java" line="361">

---

# Step 2: Revoke Original Shared Access

After successfully copying the shared access, `revokeSharedAccess` is invoked to remove the shared access from the original location, securing the resource's integrity and access control.

```java
        revokeSharedAccess(bucket, location, new ResourceLinkCollection(Set.of(new ResourceLink(source.getUrl()))));
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYWktZGlhbC1jb3JlJTNBJTNBZXBhbQ==" repo-name="ai-dial-core"><sup>Powered by [Swimm](/)</sup></SwmMeta>
