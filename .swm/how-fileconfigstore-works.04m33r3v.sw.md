---
title: How FileConfigStore Works
---
This document will cover the topic of 'FileConfigStore' in the ai-dial-core project. We'll cover:

1. What is 'FileConfigStore' and where it is implemented
2. The main classes/functions/files that are related to 'FileConfigStore'

<SwmSnippet path="/src/main/java/com/epam/aidial/core/config/FileConfigStore.java" line="20">

---

# What is 'FileConfigStore' and where it is implemented

`FileConfigStore` is a class that implements the `ConfigStore` interface. It is responsible for loading and storing configuration data from files. The configuration data includes routes, models, addons, assistants, applications, and roles. The `load` method is used to load the configuration data, and the `loadConfig` method is used to load the configuration data from files.

```java
@Slf4j
public final class FileConfigStore implements ConfigStore {

    private final String[] paths;
    private volatile Config config;
    private final ApiKeyStore apiKeyStore;

    public FileConfigStore(Vertx vertx, JsonObject settings, ApiKeyStore apiKeyStore) {
        this.apiKeyStore = apiKeyStore;
        this.paths = settings.getJsonArray("files")
                .stream().map(path -> (String) path).toArray(String[]::new);

        long period = settings.getLong("reload");
        load(true);
        vertx.setPeriodic(period, period, event -> load(false));
    }

    @Override
    public Config load() {
        return config;
    }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/config/FileConfigStore.java" line="27">

---

# The main classes/functions/files that are related to 'FileConfigStore'

The constructor of `FileConfigStore` takes in a `Vertx` instance, a `JsonObject` for settings, and an `ApiKeyStore`. It initializes the paths from the settings, loads the configuration data, and sets a periodic task to reload the configuration data.

```java
    public FileConfigStore(Vertx vertx, JsonObject settings, ApiKeyStore apiKeyStore) {
        this.apiKeyStore = apiKeyStore;
        this.paths = settings.getJsonArray("files")
                .stream().map(path -> (String) path).toArray(String[]::new);

        long period = settings.getLong("reload");
        load(true);
        vertx.setPeriodic(period, period, event -> load(false));
    }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/config/FileConfigStore.java" line="110">

---

The `loadConfig` method is used to load the configuration data from files. It creates a `JsonNode` object, reads the data from the files into the `JsonNode` object, and then converts the `JsonNode` object into a `Config` object.

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


