---
title: ModelType in the Application
---
This document will cover the topic of 'ModelType' in the ai-dial-core project. We'll cover:

1. What is 'ModelType' and its implementation.
2. The main classes/functions/files that are related to 'ModelType'.

<SwmSnippet path="/src/main/java/com/epam/aidial/core/config/ModelType.java" line="3">

---

# What is 'ModelType' and its implementation

`ModelType` is an enumeration defined in the `ModelType.java` file. It has three constants: CHAT, COMPLETION, and EMBEDDING. These constants represent the different types of models that can be used in the application.

```java
public enum ModelType {
    CHAT, COMPLETION, EMBEDDING
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/config/Model.java" line="13">

---

# The main classes/functions/files that are related to 'ModelType'

`Model` is a class that extends `Deployment` and contains a `ModelType` field. This field is used to specify the type of the model. The `Model` class also contains other fields such as `tokenizerModel`, `limits`, `pricing`, `upstreams`, and `overrideName`.

```java
    private ModelType type;
    private String tokenizerModel;
    private TokenLimits limits;
    private Pricing pricing;
    private List<Upstream> upstreams = List.of();
    // if it's set then the model name is overridden with that name in the request body to the model adapter
    private String overrideName;
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*


