---
title: Overview of ProxyContext
---
This document will cover the topic of 'ProxyContext' in the ai-dial-core repository. We'll cover:

1. What is 'ProxyContext' and where it is implemented
2. The main classes/functions/files that are related to 'ProxyContext'

<SwmSnippet path="/src/main/java/com/epam/aidial/core/ProxyContext.java" line="28">

---

# What is 'ProxyContext' and where it is implemented

`ProxyContext` is a class that encapsulates the context of a proxy request. It contains information about the request, response, API key data, trace IDs, and other related data. It is implemented in the file `src/main/java/com/epam/aidial/core/ProxyContext.java`.

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

<SwmSnippet path="/src/main/java/com/epam/aidial/core/ProxyContext.java" line="71">

---

# Main classes/functions/files related to 'ProxyContext'

The `ProxyContext` constructor initializes the context with the provided configuration, request, API key data, extracted claims, trace ID, and span ID. It also initializes the extracted claims if the API key data has a per request key.

```java
    public ProxyContext(Config config, HttpServerRequest request, ApiKeyData apiKeyData, ExtractedClaims extractedClaims, String traceId, String spanId) {
        this.config = config;
        this.apiKeyData = apiKeyData;
        this.request = request;
        this.response = request.response();
        this.requestTimestamp = System.currentTimeMillis();
        this.key = apiKeyData.getOriginalKey();
        if (apiKeyData.getPerRequestKey() != null) {
            initExtractedClaims(apiKeyData.getExtractedClaims());
            this.traceId = apiKeyData.getTraceId();
            this.parentSpanId = apiKeyData.getSpanId();
            this.sourceDeployment = apiKeyData.getSourceDeployment();
        } else {
            initExtractedClaims(extractedClaims);
            this.traceId = traceId;
            this.parentSpanId = null;
            this.sourceDeployment = null;
        }
        this.spanId = spanId;
    }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/ProxyContext.java" line="105">

---

The `respond` method is used to send a response with the given HTTP status and object. The object is converted to a JSON string and set as the response body.

```java
    @SneakyThrows
    public Future<Void> respond(HttpStatus status, Object object) {
        String json = ProxyUtil.MAPPER.writeValueAsString(object);
        response.putHeader(HttpHeaders.CONTENT_TYPE, Proxy.HEADER_CONTENT_TYPE_APPLICATION_JSON);
        return respond(status, json);
    }
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*


