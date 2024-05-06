---
title: 'Interactions Between Proxy Services '
---
This document explores the interactions between various proxy services within the ai-dial-core project. Key areas covered include:

1. How services are structured and interact within the Proxy class.
2. The role of specific services in managing data and security.

<SwmSnippet path="/src/main/java/com/epam/aidial/core/Proxy.java" line="18">

---

# Proxy Services Structure

The Proxy class integrates various services such as `ResourceOperationService`, `ResourceService`, `ShareService`, and others, facilitating their interaction to manage different aspects of data handling and operations.

```java
import com.epam.aidial.core.service.ResourceOperationService;
import com.epam.aidial.core.service.ResourceService;
import com.epam.aidial.core.service.ShareService;
import com.epam.aidial.core.storage.BlobStorage;
import com.epam.aidial.core.token.TokenStatsTracker;
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/Proxy.java" line="74">

---

# Data and Security Management

Services like `EncryptionService`, `ResourceService`, and `AccessService` are crucial for ensuring data security and access control within the proxy, highlighting their interconnected roles in the system's architecture.

```java
    private final AccessTokenValidator tokenValidator;
    private final BlobStorage storage;
    private final EncryptionService encryptionService;
    private final ApiKeyStore apiKeyStore;
    private final TokenStatsTracker tokenStatsTracker;
    private final ResourceService resourceService;
    private final InvitationService invitationService;
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

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYWktZGlhbC1jb3JlJTNBJTNBZXBhbQ==" repo-name="ai-dial-core"><sup>Powered by [Swimm](/)</sup></SwmMeta>
