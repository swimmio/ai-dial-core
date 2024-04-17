---
title: 'The Config '
---
This document will cover the topic of 'Config' in the ai-dial-core project. We'll cover:

1. What 'Config' is and where it is implemented.
2. The main classes/functions/files that are related to 'Config'.

<SwmSnippet path="/src/main/java/com/epam/aidial/core/config/Config.java" line="10">

---

# What is 'Config' and where is it implemented

The 'Config' class is a central part of the configuration system in ai-dial-core. It holds various configuration details such as routes, models, addons, applications, assistants, keys, and roles. It also provides a method 'selectDeployment' to select a specific deployment based on its ID.

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

<SwmSnippet path="/src/main/java/com/epam/aidial/core/config/FileConfigStore.java" line="110">

---

# Main classes/functions/files related to 'Config'

The 'loadConfig' method in the 'FileConfigStore' class is responsible for loading the configuration from a JSON file. It uses the 'ProxyUtil.MAPPER' to read the JSON file and convert it into a 'Config' object.

```java
    private Config loadConfig() throws Exception {
        JsonNode tree = ProxyUtil.MAPPER.createObjectNode();

        for (String path : paths) {
            try (InputStream stream = openStream(path)) {
                tree = ProxyUtil.MAPPER.readerForUpdating(tree).readTree(stream);
            }
        }

        return ProxyUtil.MAPPER.convertValue(tree, Config.class);
    }
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*


