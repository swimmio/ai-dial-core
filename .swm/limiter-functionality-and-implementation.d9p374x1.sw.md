---
title: Limiter Functionality and Implementation
---
This document will explore the functionality and implementation of Limiter in the ai-dial-core project. We'll cover:

1. What Limiter does in the system.
2. How Limiter is used across different components.

# Limiter Functionality

Limiter in the ai-dial-core project is designed to manage and enforce rate limits on various resources and operations. It ensures that the system adheres to predefined usage thresholds, preventing overuse and ensuring fair resource distribution among users.

<SwmSnippet path="/src/main/java/com/epam/aidial/core/limiter/RateLimiter.java" line="37">

---

# Implementation of Limiter

The `increase` method in `RateLimiter` is responsible for incrementing the usage count of tokens. It checks if the resource service is available and updates the token limit accordingly.

```java
    public Future<Void> increase(ProxyContext context) {
        try {
            // skip checking limits if redis is not available
            if (resourceService == null) {
                return Future.succeededFuture();
            }

            TokenUsage usage = context.getTokenUsage();

            if (usage == null || usage.getTotalTokens() <= 0) {
                return Future.succeededFuture();
            }

            String tokensPath = getPathToTokens(context.getDeployment().getName());
            ResourceDescription resourceDescription = getResourceDescription(context, tokensPath);
            return vertx.executeBlocking(() -> updateTokenLimit(resourceDescription, usage.getTotalTokens()));
        } catch (Throwable e) {
            return Future.failedFuture(e);
        }
    }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/limiter/RateLimiter.java" line="58">

---

The `limit` method handles the actual enforcement of rate limits. It retrieves the current limit settings, checks if the usage exceeds the limits, and returns a result indicating whether the operation is allowed or blocked.

```java
    public Future<RateLimitResult> limit(ProxyContext context) {
        try {
            // skip checking limits if redis is not available
            if (resourceService == null) {
                return Future.succeededFuture(RateLimitResult.SUCCESS);
            }
            Key key = context.getKey();
            String deploymentName = context.getDeployment().getName();
            Limit limit;
            if (key == null) {
                limit = getLimitByUser(context, deploymentName);
            } else {
                limit = getLimitByApiKey(context, deploymentName);
            }

            if (limit == null || !limit.isPositive()) {
                if (limit == null) {
                    log.warn("Limit is not found for deployment: {}", deploymentName);
                } else {
                    log.warn("Limit must be positive for deployment: {}", deploymentName);
                }
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYWktZGlhbC1jb3JlJTNBJTNBZXBhbQ==" repo-name="ai-dial-core"><sup>Powered by [Swimm](/)</sup></SwmMeta>
