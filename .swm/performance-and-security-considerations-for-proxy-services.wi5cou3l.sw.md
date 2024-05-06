---
title: Performance and Security Considerations for Proxy Services
---
This document will explore the key performance and security considerations for Proxy Services in a distributed environment, focusing on:

<SwmSnippet path="/src/main/java/com/epam/aidial/core/Proxy.java" line="63">

---

# Performance Considerations

The Proxy service restricts the allowed HTTP methods to GET, POST, PUT, and DELETE to enhance performance by limiting the types of requests handled.

```java
    public static final int REQUEST_BODY_MAX_SIZE_BYTES = 16 * 1024 * 1024;
    public static final int FILES_REQUEST_BODY_MAX_SIZE_BYTES = 512 * 1024 * 1024;

    private static final Set<HttpMethod> ALLOWED_HTTP_METHODS = Set.of(HttpMethod.GET, HttpMethod.POST, HttpMethod.PUT, HttpMethod.DELETE);
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/controller/DeploymentController.java" line="62">

---

# Security Considerations

Security is enforced through access control checks in the `hasAccess` method, which verifies user roles and assesses limits based on deployment configurations.

```java
    public static boolean hasAccess(ProxyContext context, Deployment deployment) {
        return hasAssessByLimits(context, deployment) && hasAccessByUserRoles(context, deployment);
    }
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYWktZGlhbC1jb3JlJTNBJTNBZXBhbQ==" repo-name="ai-dial-core"><sup>Powered by [Swimm](/)</sup></SwmMeta>
