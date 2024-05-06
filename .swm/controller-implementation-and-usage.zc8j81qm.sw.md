---
title: Controller Implementation and Usage
---
This document will cover the implementation and usage of Controllers in the ai-dial-core project, specifically focusing on:

# Overview of Controllers

Controllers in the ai-dial-core project are designed to handle HTTP requests and orchestrate responses based on the application's business logic. They are crucial for managing the data flow between the server and the client-side of the application.

<SwmSnippet path="/src/main/java/com/epam/aidial/core/controller/Controller.java" line="7">

---

# Implementation of Controllers

The `Controller` interface defines a common structure for all controllers, requiring them to implement the `handle` method which returns a `Future` object. This method is essential for asynchronous processing of HTTP requests.

```java
/**
 *  Common interface for HTTP controllers.
 *  <p>
 *      Note. The interface must extends {@link Serializable} for exposing internal lambda fields in Unit tests.
 *  </p>
 */
public interface Controller extends Serializable {

    /**
     * The controller must return non-null instance of {@link Future}.
     */
    Future<?> handle() throws Exception;
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/controller/RouteController.java" line="34">

---

# Usage of Controllers

The `RouteController` implements the `Controller` interface and provides an example of handling HTTP requests by selecting routes and managing responses. It demonstrates how controllers interact with other components like `Proxy` and `ProxyContext` to perform their tasks.

```java
public class RouteController implements Controller {

    private final Proxy proxy;
    private final ProxyContext context;

    @Override
    public Future<?> handle() {
        Route route = selectRoute();
        if (route == null) {
            context.respond(HttpStatus.BAD_GATEWAY, "No route");
            return Future.succeededFuture();
        }

        Route.Response response = route.getResponse();
        if (response == null) {
            UpstreamProvider upstreamProvider = new RouteEndpointProvider(route);
            UpstreamRoute upstreamRoute = proxy.getUpstreamBalancer().balance(upstreamProvider);

            if (!upstreamRoute.hasNext()) {
                context.respond(HttpStatus.BAD_GATEWAY, "No route");
                return Future.succeededFuture();
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYWktZGlhbC1jb3JlJTNBJTNBZXBhbQ==" repo-name="ai-dial-core"><sup>Powered by [Swimm](/)</sup></SwmMeta>
