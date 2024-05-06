---
title: 'Configuration '
---
This document will cover the Configuration in the ai-dial-core project, focusing on its implementation and usage across various components:

<SwmSnippet path="/src/main/java/com/epam/aidial/core/config/Config.java" line="10">

---

# Overview of Configuration

The `Config` class is central in managing configurations throughout the ai-dial-core project. It holds configurations for routes, models, addons, applications, assistants, keys, and roles. The method `selectDeployment` dynamically selects the deployment based on the provided ID, demonstrating the class's role in handling various configurations dynamically.

```java
@Data
@JsonIgnoreProperties(ignoreUnknown = true)
public class Config {
    public static final String ASSISTANT = "assistant";

    // maintain the order of routes defined in the config
    private LinkedHashMap<String, Route> routes = new LinkedHashMap<>();
    private Map<String, Model> models = Map.of();
    private Map<String, Addon> addons = Map.of();
    private Map<String, Application> applications = Map.of();
    private Assistants assistant = new Assistants();
    private Map<String, Key> keys = new HashMap<>();
    private Map<String, Role> roles = new HashMap<>();


    public Deployment selectDeployment(String deploymentId) {
        Application application = applications.get(deploymentId);
        if (application != null) {
            return application;
        }

```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/controller/AssistantController.java" line="22">

---

# Usage of Configuration

In `AssistantController`, `Config` is used to retrieve specific assistant configurations. The method `getAssistant` fetches an assistant's configuration using the assistant ID, which is a practical example of how configurations are accessed and utilized in the application's operational context.

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

    public Future<?> getAssistants() {
        Config config = context.getConfig();
        List<AssistantData> assistants = new ArrayList<>();
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYWktZGlhbC1jb3JlJTNBJTNBZXBhbQ==" repo-name="ai-dial-core"><sup>Powered by [Swimm](/)</sup></SwmMeta>
