---
title: Overview of ConfigStore
---
This document will cover the topic of ConfigStore in the ai-dial-core project. We'll cover:

1. What is ConfigStore and where it is implemented
2. The main classes and functions related to ConfigStore

<SwmSnippet path="/src/main/java/com/epam/aidial/core/config/ConfigStore.java" line="3">

---

# What is ConfigStore

`ConfigStore` is an interface that provides a method `load()` which returns an immutable config. It is allowed to return not up-to-date config for some period of time e.g. 1 min.

```java
public interface ConfigStore {

    /**
     * Allowed to return not up-to-date config for some period of time e.g. 1 min.
     *
     * @return immutable config.
     */
    Config load();

}
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/config/FileConfigStore.java" line="20">

---

# Main classes and functions related to ConfigStore

`FileConfigStore` is a class that implements `ConfigStore`. It provides the implementation of the `load()` method from `ConfigStore` interface. It also has a `loadConfig()` method which loads the configuration from a file and a `load()` method which loads the configuration and sets it to the `config` field.

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

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*


