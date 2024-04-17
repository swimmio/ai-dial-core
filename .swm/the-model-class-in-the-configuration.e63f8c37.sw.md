---
title: The Model Class in the Configuration
---
This document will cover the topic of 'Model' in the ai-dial-core project. We'll cover:

1. What 'Model' is and where it is implemented.
2. The main classes/functions/files related to 'Model'.

<SwmSnippet path="/src/main/java/com/epam/aidial/core/config/Model.java" line="9">

---

# What is 'Model' and where is it implemented

`Model` is a class that extends `Deployment`. It contains several fields including `ModelType type`, `String tokenizerModel`, `TokenLimits limits`, `Pricing pricing`, `List<Upstream> upstreams`, and `String overrideName`. This class is defined in `Model.java`.

```java
@Data
@Accessors(chain = true)
@EqualsAndHashCode(callSuper = true)
public class Model extends Deployment {
    private ModelType type;
    private String tokenizerModel;
    private TokenLimits limits;
    private Pricing pricing;
    private List<Upstream> upstreams = List.of();
    // if it's set then the model name is overridden with that name in the request body to the model adapter
    private String overrideName;
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/config/Config.java" line="17">

---

# Main classes/functions/files related to 'Model'

`Model` is used in the `Config` class. The `Config` class has a `Map<String, Model> models` field which stores the models.

```java
    private Map<String, Model> models = Map.of();
    private Map<String, Addon> addons = Map.of();
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*


