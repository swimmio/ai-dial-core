---
title: Performance Considerations in the Proxy Component
---
This document will cover the performance considerations taken in the ai-dial-core codebase, focusing on the use of precompiled patterns for request routing.

<SwmSnippet path="/src/main/java/com/epam/aidial/core/controller/ControllerSelector.java" line="44">

---

# Use of Precompiled Patterns

The `PATTERN_RATE_RESPONSE`, `PATTERN_TOKENIZE`, and `PATTERN_TRUNCATE_PROMPT` are precompiled patterns used for matching incoming request paths. Precompiling these patterns improves performance by avoiding the overhead of repeatedly compiling the same regular expressions.

```java
    private static final Pattern PATTERN_RATE_RESPONSE = Pattern.compile("^/+v1/([^/]+)/rate$");
    private static final Pattern PATTERN_TOKENIZE = Pattern.compile("^/+v1/deployments/([^/]+)/tokenize$");
    private static final Pattern PATTERN_TRUNCATE_PROMPT = Pattern.compile("^/+v1/deployments/([^/]+)/truncate_prompt$");
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/controller/ControllerSelector.java" line="221">

---

Here, the precompiled `PATTERN_RATE_RESPONSE` is used to match the incoming request path. If the pattern matches, the corresponding controller is selected to handle the request, thus improving routing efficiency.

```java
        match = match(PATTERN_RATE_RESPONSE, path);
        if (match != null) {
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*


