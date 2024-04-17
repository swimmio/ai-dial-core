---
title: Deployment in the Application
---
This document will cover the concept of 'Deployment' within the ai-dial-core project. We'll cover:

1. What 'Deployment' is and where it is implemented.
2. The main classes and functions related to 'Deployment'.

<SwmSnippet path="/src/main/java/com/epam/aidial/core/config/Deployment.java" line="8">

---

# What is 'Deployment'?

The 'Deployment' is an abstract class that serves as a base for different types of deployments in the application. It contains common properties like 'name', 'endpoint', 'displayName', 'displayVersion', 'iconUrl', 'description', 'userRoles', 'forwardAuthToken', 'features', 'inputAttachmentTypes', and 'maxInputAttachments'.

```java
@Data
public abstract class Deployment {
    private String name;
    private String endpoint;
    private String displayName;
    private String displayVersion;
    private String iconUrl;
    private String description;
    private Set<String> userRoles = Set.of();
    /**
     * Forward Http header with authorization token when request is sent to deployment.
     * Authorization token is NOT forwarded by default.
     */
    private boolean forwardAuthToken = false;
    private Features features;
    private List<String> inputAttachmentTypes;
    private Integer maxInputAttachments;
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/config/Config.java" line="25">

---

# Main classes and functions related to 'Deployment'

The 'selectDeployment' method in the 'Config' class is used to select a specific deployment by its ID. It first checks if the deployment ID matches an 'Application', then a 'Model', and finally an 'Assistant'. If a match is found, it returns the corresponding deployment.

```java
    public Deployment selectDeployment(String deploymentId) {
        Application application = applications.get(deploymentId);
        if (application != null) {
            return application;
        }

        Model model = models.get(deploymentId);
        if (model != null) {
            return model;
        }

        Assistants assistants = assistant;
        return assistants.getAssistants().get(deploymentId);
    }
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*


