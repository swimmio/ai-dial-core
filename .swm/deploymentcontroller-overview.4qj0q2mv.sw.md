---
title: DeploymentController Overview
---
This document will cover the DeploymentController in the ai-dial-core project. We'll cover:

1. What is the DeploymentController and where it is implemented
2. The main classes/functions/files that are related to DeploymentController

<SwmSnippet path="/src/main/java/com/epam/aidial/core/controller/DeploymentController.java" line="22">

---

# What is the DeploymentController and where it is implemented

The `DeploymentController` is a class that manages the deployment of models in the application. It provides methods to get a specific deployment, get all deployments, check access to a deployment, and create a deployment. It is implemented in the `DeploymentController.java` file.

```java
@RequiredArgsConstructor
public class DeploymentController {

    private final ProxyContext context;

    public Future<?> getDeployment(String deploymentId) {
        Config config = context.getConfig();
        Model model = config.getModels().get(deploymentId);

        if (model == null) {
            return context.respond(HttpStatus.NOT_FOUND);
        }

        if (!DeploymentController.hasAccess(context, model)) {
            return context.respond(HttpStatus.FORBIDDEN);
        }

        DeploymentData data = createDeployment(model);
        context.respond(HttpStatus.OK, data);
        return Future.succeededFuture();
    }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/controller/DeploymentController.java" line="27">

---

# The main classes/functions/files that are related to DeploymentController

The `getDeployment` method retrieves a specific deployment by its ID. It first gets the model associated with the deployment ID from the application's configuration. If the model is not found or the user does not have access to it, it responds with an appropriate HTTP status. Otherwise, it creates a `DeploymentData` object for the model and responds with it.

```java
    public Future<?> getDeployment(String deploymentId) {
        Config config = context.getConfig();
        Model model = config.getModels().get(deploymentId);

        if (model == null) {
            return context.respond(HttpStatus.NOT_FOUND);
        }

        if (!DeploymentController.hasAccess(context, model)) {
            return context.respond(HttpStatus.FORBIDDEN);
        }

        DeploymentData data = createDeployment(model);
        context.respond(HttpStatus.OK, data);
        return Future.succeededFuture();
    }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/controller/DeploymentController.java" line="44">

---

The `getDeployments` method retrieves all deployments that the user has access to. It iterates over all models in the application's configuration, checks access for each one, and adds a `DeploymentData` object for it to a list if access is granted. It then responds with the list of `DeploymentData` objects.

```java
    public Future<?> getDeployments() {
        Config config = context.getConfig();
        List<DeploymentData> deployments = new ArrayList<>();

        for (Model model : config.getModels().values()) {
            if (hasAccess(context, model)) {
                DeploymentData deployment = createDeployment(model);
                deployments.add(deployment);
            }
        }

        ListData<DeploymentData> list = new ListData<>();
        list.setData(deployments);

        context.respond(HttpStatus.OK, list);
        return Future.succeededFuture();
    }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/controller/DeploymentController.java" line="62">

---

The `hasAccess` method checks if the user has access to a deployment. It checks both access by limits and access by user roles. If both checks pass, the user has access to the deployment.

```java
    public static boolean hasAccess(ProxyContext context, Deployment deployment) {
        return hasAssessByLimits(context, deployment) && hasAccessByUserRoles(context, deployment);
    }

    public static boolean hasAssessByLimits(ProxyContext context, Deployment deployment) {
        Key key = context.getKey();
        if (key == null || key.getRole() == null) {
            return true;
        }
        Role keyRole = context.getConfig().getRoles().get(key.getRole());
        if (keyRole == null) {
            return false;
        }

        Limit keyLimit = keyRole.getLimits().get(deployment.getName());
        if (keyLimit == null || !keyLimit.isPositive()) {
            return false;
        }

        return true;
    }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/controller/DeploymentController.java" line="95">

---

The `createDeployment` method creates a `DeploymentData` object for a model. It sets the ID and model of the `DeploymentData` object to the name of the model.

```java
    private static DeploymentData createDeployment(Model model) {
        DeploymentData deployment = new DeploymentData();
        deployment.setId(model.getName());
        deployment.setModel(model.getName());
        return deployment;
    }
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*


