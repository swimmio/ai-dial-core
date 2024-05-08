---
title: Proxy Services Implementation and Usage
---
This document will explore the implementation and functionality of Proxy Services within the ai-dial-core project, specifically focusing on how they are utilized to manage various service interactions and data handling.

# Overview of Proxy Services

Proxy Services in the ai-dial-core project are designed to handle interactions between the server's API and various services like ShareService, PublicationService, and ResourceOperationService. These services facilitate operations such as sharing resources, publishing content, and performing resource-specific operations, making the Proxy a central component in managing service requests and data flow.

<SwmSnippet path="/src/main/java/com/epam/aidial/core/Proxy.java" line="81">

---

# Implementation of Proxy Services

Here, Proxy Services are implemented as fields within the Proxy class, each representing a different aspect of the application's functionality such as access management, resource sharing, and publication.

```java
    private final ShareService shareService;
    private final PublicationService publicationService;
    private final AccessService accessService;
    private final LockService lockService;
    private final ResourceOperationService resourceOperationService;
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYWktZGlhbC1jb3JlJTNBJTNBc3dpbW1pbw==" repo-name="ai-dial-core"><sup>Powered by [Swimm](/)</sup></SwmMeta>
