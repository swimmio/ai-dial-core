---
title: Overview of Assistant in the Application
---
This document will cover the topic of 'Assistant' in the ai-dial-core project. We'll cover:

1. What 'Assistant' is and where it is implemented.
2. The main classes/functions/files related to 'Assistant'.

<SwmSnippet path="/src/main/java/com/epam/aidial/core/config/Assistant.java" line="9">

---

# What is 'Assistant' and where is it implemented

The 'Assistant' class extends the 'Deployment' class and includes a 'prompt' and a list of 'addons'. It is a data model that represents an assistant in the application.

```java
@Data
@Accessors(chain = true)
@EqualsAndHashCode(callSuper = true)
public class Assistant extends Deployment {
    private String prompt;
    private List<String> addons = List.of();
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/config/Assistants.java" line="8">

---

# Main classes/functions/files related to 'Assistant'

The 'Assistants' class holds a map of 'Assistant' objects. It is used to manage and access the different assistants in the application.

```java
@Data
public class Assistants {
    private String endpoint;
    private Features features;
    private Map<String, Assistant> assistants = new HashMap<>();
}
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*


