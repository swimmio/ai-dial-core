---
title: API
---
This document will cover the following topics related to the API in the ai-dial-core repository:

1. Overview of the API and its endpoints
2. Where to find the list of endpoints
3. How to use Swagger and Postman for API documentation

<SwmSnippet path="/src/main/java/com/epam/aidial/core/controller/DeploymentPostController.java" line="480">

---

# Overview of the API

The `isValidDeploymentApi` method in the `DeploymentPostController` class is a good starting point to understand the API. It checks if a given deployment API is valid. The API supports three types of models: `COMPLETION`, `CHAT`, and `EMBEDDING`. The method returns `false` if the type is `null`, meaning the API does not support the given model type.

```java
    private static boolean isValidDeploymentApi(Deployment deployment, String deploymentApi) {
        ModelType type = switch (deploymentApi) {
            case "completions" -> ModelType.COMPLETION;
            case "chat/completions" -> ModelType.CHAT;
            case "embeddings" -> ModelType.EMBEDDING;
            default -> null;
        };

        if (type == null) {
            return false;
        }

        // Models support all APIs
        if (deployment instanceof Model model) {
            return type == model.getType();
        }

        // Assistants and applications only support chat API
        return type == ModelType.CHAT;
    }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/ProxyContext.java" line="28">

---

# Finding the List of Endpoints

The `ProxyContext` class provides a lot of information about the current request, including the API key, request and response objects, and more. This class is used throughout the codebase, indicating that it plays a crucial role in handling requests and responses.

```java
@Slf4j
@Getter
@Setter
public class ProxyContext {

    private static final int LOG_MAX_ERROR_LENGTH = 200;

    private final Config config;
    // API key of root requester
    private final Key key;
    private final HttpServerRequest request;
    private final HttpServerResponse response;
    // OpenTelemetry trace ID
    private final String traceId;
    // API key data associated with the current request
    private final ApiKeyData apiKeyData;
    // OpenTelemetry span ID created by the Core
    private final String spanId;
    // OpenTelemetry parent span ID created by the Core
    private final String parentSpanId;
    // deployment name of the source(application/assistant/model) associated with the current request
```

---

</SwmSnippet>

# Swagger

The API documentation is not directly available via Swagger in this repository. However, the `ProxyContext` class and the `DeploymentPostController` class provide a good starting point to understand how the API works.

# Postman

Similar to Swagger, the API documentation is not directly available via Postman in this repository. However, the `ProxyContext` class and the `DeploymentPostController` class provide a good starting point to understand how the API works.

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*


