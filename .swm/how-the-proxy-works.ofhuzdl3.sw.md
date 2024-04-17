---
title: 'How the Proxy works '
---
This document will cover the topic of 'Proxy' in the ai-dial-core repository. We'll cover:

1. What is a 'Proxy' in ai-dial-core
2. The main classes/functions/files that are related to 'Proxy'.

# What is a 'Proxy' in ai-dial-core

In the ai-dial-core repository, a 'Proxy' is a key component that handles HTTP requests. It is implemented in the `Proxy.java` file located in the `src/main/java/com/epam/aidial/core` directory. The Proxy class is responsible for handling incoming HTTP requests, checking the HTTP version and method, and responding accordingly. It also handles errors and provides responses with appropriate HTTP status codes.

<SwmSnippet path="/src/main/java/com/epam/aidial/core/Proxy.java" line="47">

---

# Main classes/functions/files related to 'Proxy'

This is the main Proxy class. It defines several constants used in HTTP requests and responses, such as HTTP methods, headers, and content types. It also contains the `handleRequest` method which is responsible for handling incoming HTTP requests.

```java
@Slf4j
@Getter
@RequiredArgsConstructor
public class Proxy implements Handler<HttpServerRequest> {

    public static final String HEALTH_CHECK_PATH = "/health";
    public static final String VERSION_PATH = "/version";

    public static final String HEADER_API_KEY = "API-KEY";
    public static final String HEADER_JOB_TITLE = "X-JOB-TITLE";
    public static final String HEADER_CONVERSATION_ID = "X-CONVERSATION-ID";
    public static final String HEADER_UPSTREAM_ENDPOINT = "X-UPSTREAM-ENDPOINT";
    public static final String HEADER_UPSTREAM_KEY = "X-UPSTREAM-KEY";
    public static final String HEADER_UPSTREAM_ATTEMPTS = "X-UPSTREAM-ATTEMPTS";
    public static final String HEADER_CONTENT_TYPE_APPLICATION_JSON = "application/json";

    public static final int REQUEST_BODY_MAX_SIZE_BYTES = 16 * 1024 * 1024;
    public static final int FILES_REQUEST_BODY_MAX_SIZE_BYTES = 512 * 1024 * 1024;

    private static final Set<HttpMethod> ALLOWED_HTTP_METHODS = Set.of(HttpMethod.GET, HttpMethod.POST, HttpMethod.PUT, HttpMethod.DELETE);

```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/util/ProxyUtil.java" line="27">

---

The `ProxyUtil` class provides utility methods used by the Proxy class. It includes methods for copying headers, calculating content length, and converting objects to JSON.

```java
@UtilityClass
@Slf4j
public class ProxyUtil {

    public static final JsonMapper MAPPER = JsonMapper.builder()
            .enable(MapperFeature.ACCEPT_CASE_INSENSITIVE_ENUMS)
            .build();

    private static final MultiMap HOP_BY_HOP_HEADERS = MultiMap.caseInsensitiveMultiMap()
            .add(HttpHeaders.CONNECTION, "whatever")
            .add(HttpHeaders.KEEP_ALIVE, "whatever")
            .add(HttpHeaders.HOST, "whatever")
            .add(HttpHeaders.PROXY_AUTHENTICATE, "whatever")
            .add(HttpHeaders.PROXY_AUTHORIZATION, "whatever")
            .add("te", "whatever")
            .add("trailer", "whatever")
            .add(HttpHeaders.TRANSFER_ENCODING, "whatever")
            .add(HttpHeaders.UPGRADE, "whatever")
            .add(HttpHeaders.CONTENT_LENGTH, "whatever")
            .add(HttpHeaders.ACCEPT_ENCODING, "whatever")
            .add(Proxy.HEADER_API_KEY, "whatever");
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/ProxyContext.java" line="28">

---

The `ProxyContext` class holds the context for a proxy request. It contains information about the request, response, API key data, and other related data.

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

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*


