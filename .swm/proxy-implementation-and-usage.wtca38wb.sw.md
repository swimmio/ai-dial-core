---
title: Proxy Implementation and Usage
---
This document will cover the implementation and usage of Proxy in the ai-dial-core project, focusing on:

# Overview of Proxy

Proxy in ai-dial-core acts as an intermediary for requests from clients seeking resources from other servers. It handles HTTP requests, determines the appropriate actions based on the request details, and forwards the request to the appropriate upstream server or service.

<SwmSnippet path="/src/main/java/com/epam/aidial/core/Proxy.java" line="88">

---

# Implementation of Proxy

The `handle` method in the Proxy class is the entry point for processing incoming HTTP requests. It delegates to `handleRequest` for further processing based on the HTTP method and headers.

```java
    @Override
    public void handle(HttpServerRequest request) {
        try {
            handleRequest(request);
        } catch (Throwable e) {
            handleError(e, request);
        }
    }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/ProxyContext.java" line="105">

---

# Usage of ProxyContext

The `respond` method in ProxyContext is used to send responses back to the client. It serializes the response object to JSON, sets the appropriate headers, and sends the HTTP response.

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

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYWktZGlhbC1jb3JlJTNBJTNBc3dpbW1pbw==" repo-name="ai-dial-core"><sup>Powered by [Swimm](/)</sup></SwmMeta>
