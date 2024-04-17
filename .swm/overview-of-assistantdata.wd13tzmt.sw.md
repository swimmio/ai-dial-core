---
title: Overview of AssistantData
---
This document will cover the topic of 'AssistantData' in the ai-dial-core repository. We'll cover:

1. What is 'AssistantData' and where it is implemented.
2. The main classes/functions/files that are related to 'AssistantData'.

<SwmSnippet path="/src/main/java/com/epam/aidial/core/data/AssistantData.java" line="11">

---

# What is 'AssistantData' and where it is implemented

`AssistantData` is a class that extends `DeploymentData`. It is annotated with `@Data`, `@EqualsAndHashCode`, `@JsonInclude`, and `@JsonNaming` annotations. It has a private field `addons` of type `List<String>`. In the constructor, it sets the object type to 'assistant' and scale settings to null.

```java
@Data
@EqualsAndHashCode(callSuper = true)
@JsonInclude(JsonInclude.Include.NON_NULL)
@JsonNaming(PropertyNamingStrategies.SnakeCaseStrategy.class)
public class AssistantData extends DeploymentData {
    private List<String> addons;

    {
        setObject("assistant");
        setScaleSettings(null);
    }
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/controller/AssistantController.java" line="34">

---

# The main classes/functions/files that are related to 'AssistantData'

`AssistantData` is used in the `AssistantController` class. It is used to create an assistant and respond with the created assistant data. It is also used to create a list of assistants and set this list to the `ListData` object.

```java
        AssistantData data = createAssistant(assistant);
        context.respond(HttpStatus.OK, data);
        return Future.succeededFuture();
    }

    public Future<?> getAssistants() {
        Config config = context.getConfig();
        List<AssistantData> assistants = new ArrayList<>();

        for (Assistant assistant : config.getAssistant().getAssistants().values()) {
            if (!ASSISTANT.equals(assistant.getName()) && DeploymentController.hasAccess(context, assistant)) {
                AssistantData data = createAssistant(assistant);
                assistants.add(data);
            }
        }

        ListData<AssistantData> list = new ListData<>();
        list.setData(assistants);
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*


