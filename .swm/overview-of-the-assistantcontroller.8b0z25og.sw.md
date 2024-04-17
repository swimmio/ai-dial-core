---
title: Overview of the AssistantController
---
This document will cover the AssistantController class in the ai-dial-core project. We'll cover:

1. What is the AssistantController
2. Main methods in the AssistantController
3. How these methods are implemented

<SwmSnippet path="/src/main/java/com/epam/aidial/core/controller/AssistantController.java" line="17">

---

# What is the AssistantController

The AssistantController is a class that manages the operations related to Assistants. It uses the ProxyContext for its operations.

```java
@RequiredArgsConstructor
public class AssistantController {

    private final ProxyContext context;

```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/controller/AssistantController.java" line="22">

---

# Main methods in the AssistantController

The `getAssistant` method is used to fetch a specific Assistant based on the provided assistantId. It checks if the assistant exists and if the current context has access to it. If these conditions are met, it creates an AssistantData object and responds with it.

```java
    public Future<?> getAssistant(String assistantId) {
        Config config = context.getConfig();
        Assistant assistant = config.getAssistant().getAssistants().get(assistantId);

        if (assistant == null || ASSISTANT.equals(assistant.getName())) {
            return context.respond(HttpStatus.NOT_FOUND);
        }

        if (!DeploymentController.hasAccess(context, assistant)) {
            return context.respond(HttpStatus.FORBIDDEN);
        }

        AssistantData data = createAssistant(assistant);
        context.respond(HttpStatus.OK, data);
        return Future.succeededFuture();
    }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/controller/AssistantController.java" line="39">

---

The `getAssistants` method is used to fetch all Assistants. It iterates over all the Assistants, checks if the current context has access to them, and if they are not the base Assistant. It then creates an AssistantData object for each and responds with a list of them.

```java
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

        context.respond(HttpStatus.OK, list);
        return Future.succeededFuture();
    }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/controller/AssistantController.java" line="57">

---

# How these methods are implemented

The `createAssistant` method is a private method used by both `getAssistant` and `getAssistants` methods. It creates an AssistantData object from an Assistant object by copying its properties.

```java
    private static AssistantData createAssistant(Assistant assistant) {
        AssistantData data = new AssistantData();
        data.setId(assistant.getName());
        data.setAssistant(assistant.getName());
        data.setDisplayName(assistant.getDisplayName());
        data.setDisplayVersion(assistant.getDisplayVersion());
        data.setIconUrl(assistant.getIconUrl());
        data.setDescription(assistant.getDescription());
        data.setAddons(assistant.getAddons());
        data.setFeatures(DeploymentController.createFeatures(assistant.getFeatures()));
        data.setInputAttachmentTypes(assistant.getInputAttachmentTypes());
        data.setMaxInputAttachments(assistant.getMaxInputAttachments());
        return data;
    }
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*


