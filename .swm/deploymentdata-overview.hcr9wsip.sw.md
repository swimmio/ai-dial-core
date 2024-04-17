---
title: DeploymentData Overview
---
This document will cover the topic of 'DeploymentData' in the ai-dial-core project. We'll cover:

1. What is 'DeploymentData'
2. The main classes that are related to 'DeploymentData'

<SwmSnippet path="/src/main/java/com/epam/aidial/core/data/DeploymentData.java" line="10">

---

# What is 'DeploymentData'

`DeploymentData` is a class that encapsulates various properties related to a deployment, such as id, model, addon, assistant, application, and others. It also includes timestamps for creation and update, as well as features and scale settings.

```java
@Data
@JsonInclude(JsonInclude.Include.NON_NULL)
@JsonNaming(PropertyNamingStrategies.SnakeCaseStrategy.class)
public class DeploymentData {
    private String id;
    private String model;
    private String addon;
    private String assistant;
    private String application;
    private String displayName;
    private String displayVersion;
    private String iconUrl;
    private String description;
    private String owner = "organization-owner";
    private String object = "deployment";
    private String status = "succeeded";
    private long createdAt = 1672534800;
    private long updatedAt = 1672534800;
    private ScaleSettingsData scaleSettings = new ScaleSettingsData();
    private FeaturesData features = new FeaturesData();
    private List<String> inputAttachmentTypes;
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/data/ApplicationData.java" line="10">

---

# Main classes related to 'DeploymentData'

`ApplicationData` is one of the classes that extends `DeploymentData`. It sets the object type to 'application' and the scale settings to null.

```java
@EqualsAndHashCode(callSuper = true)
@JsonInclude(JsonInclude.Include.NON_NULL)
@JsonNaming(PropertyNamingStrategies.SnakeCaseStrategy.class)
public class ApplicationData extends DeploymentData {
    {
        setObject("application");
        setScaleSettings(null);
    }
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/data/AddonData.java" line="10">

---

`AddonData` is another class that extends `DeploymentData`. It sets the object type to 'addon' and the scale settings to null.

```java
@EqualsAndHashCode(callSuper = true)
@JsonInclude(JsonInclude.Include.NON_NULL)
@JsonNaming(PropertyNamingStrategies.SnakeCaseStrategy.class)
public class AddonData extends DeploymentData {
    {
        setObject("addon");
        setScaleSettings(null);
    }
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/data/AssistantData.java" line="12">

---

`AssistantData` is a class that extends `DeploymentData` and adds a list of addons. It sets the object type to 'assistant' and the scale settings to null.

```java
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

<SwmSnippet path="/src/main/java/com/epam/aidial/core/data/ModelData.java" line="10">

---

`ModelData` is a class that extends `DeploymentData` and adds additional properties specific to a model. It sets the object type to 'model' and the scale settings to null.

```java
@EqualsAndHashCode(callSuper = true)
@JsonInclude(JsonInclude.Include.NON_NULL)
@JsonNaming(PropertyNamingStrategies.SnakeCaseStrategy.class)
public class ModelData extends DeploymentData {

    private String lifecycleStatus = "generally-available";
    private CapabilitiesData capabilities = new CapabilitiesData();
    private String tokenizerModel;
    private TokenLimitsData limits;
    private PricingData pricing;

    {
        setObject("model");
        setScaleSettings(null);
    }
}
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*


