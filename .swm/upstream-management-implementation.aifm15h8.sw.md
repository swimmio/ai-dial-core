---
title: Upstream Management Implementation
---
This document will explore the implementation and usage of Upstream Management in the ai-dial-core project. We'll cover:

1. The role and functionality of Upstream Management.
2. Key implementations in the codebase.

# Upstream Management Functionality

Upstream Management in ai-dial-core is crucial for managing the routing of requests to different upstream services based on the deployment context. It involves selecting the appropriate upstream service provider and balancing the load among available services.

<SwmSnippet path="/src/main/java/com/epam/aidial/core/upstream/UpstreamProvider.java" line="7">

---

# Implementation in the Codebase

The `UpstreamProvider` interface is central to Upstream Management. It defines methods like `getName()` to get the provider's name and `getUpstreams()` to retrieve a list of upstream configurations.

```java
public interface UpstreamProvider {
    String getName();

    List<Upstream> getUpstreams();
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/upstream/UpstreamBalancer.java" line="16">

---

The `UpstreamBalancer` class uses the `UpstreamProvider` to balance the load among different upstreams. It selects an upstream based on the provided `UpstreamProvider`.

```java
    public UpstreamRoute balance(UpstreamProvider provider) {
        String name = provider.getName();
        List<Upstream> upstreams = provider.getUpstreams();
        int offset = 0;
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYWktZGlhbC1jb3JlJTNBJTNBZXBhbQ==" repo-name="ai-dial-core"><sup>Powered by [Swimm](/)</sup></SwmMeta>
