---
title: Invitation Class Implementation and Usage
---
This document will explore the functionality and implementation of Invitation in the ai-dial-core codebase:

# Overview of Invitation

Invitation in ai-dial-core is a data class used to manage access to resources. It includes properties such as `id`, `resources`, `createdAt`, and `expireAt` to handle the lifecycle and permissions of an invitation.

<SwmSnippet path="/src/main/java/com/epam/aidial/core/data/Invitation.java" line="9">

---

# Implementation of Invitation

The Invitation class is defined with annotations @Data, @AllArgsConstructor, and @NoArgsConstructor, making it a simple data holding object with automatic getters, setters, and constructors.

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Invitation {
    String id;
    Set<ResourceLink> resources;
    long createdAt;
    long expireAt;
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/service/InvitationService.java" line="47">

---

# Usage of Invitation in Services

Here, an Invitation object is created and configured in the `createInvitation` method, which sets up a new invitation with resources, creation time, and expiration time.

```java
    public Invitation createInvitation(String bucket, String location, Set<ResourceLink> resources) {
        ResourceDescription resource = ResourceDescription.fromDecoded(ResourceType.INVITATION, bucket, location, INVITATION_RESOURCE_FILENAME);
        String invitationId = generateInvitationId(resource);
        Instant creationTime = Instant.now();
        Instant expirationTime = Instant.now().plus(expirationInSeconds, ChronoUnit.SECONDS);
        Invitation invitation = new Invitation(invitationId, resources, creationTime.toEpochMilli(), expirationTime.toEpochMilli());
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/service/InvitationService.java" line="67">

---

# Retrieval and Validation

The `getInvitation` method retrieves an Invitation by its ID and checks its validity. If the invitation is not found or is expired, it returns null.

```java
    @Nullable
    public Invitation getInvitation(String invitationId) {
        ResourceDescription resource = getInvitationResource(invitationId);
        if (resource == null) {
            return null;
        }
        String resourceState = resourceService.getResource(resource);
        InvitationsMap invitations = ProxyUtil.convertToObject(resourceState, InvitationsMap.class);
        if (invitations == null) {
            return null;
        }

        Invitation invitation = invitations.getInvitations().get(invitationId);
        if (invitation == null) {
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYWktZGlhbC1jb3JlJTNBJTNBZXBhbQ==" repo-name="ai-dial-core"><sup>Powered by [Swimm](/)</sup></SwmMeta>
