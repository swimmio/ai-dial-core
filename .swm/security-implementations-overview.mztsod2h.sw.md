---
title: Security Implementations Overview
---
This document will cover the implementation and functionality of security features within the ai-dial-core project, specifically focusing on the `com.epam.aidial.core.security` package. We'll explore:

1. What Security does in the codebase
2. How Security is implemented across various services

# Security Functionality

Security in the ai-dial-core project is primarily concerned with encryption, access control, and identity verification. It ensures that sensitive data is encrypted using robust algorithms and manages access to resources based on predefined rules and roles.

<SwmSnippet path="/src/main/java/com/epam/aidial/core/security/EncryptionService.java" line="19">

---

# Implementation of Security

The `EncryptionService` class handles data encryption and decryption. It uses AES algorithm with CBC mode and PKCS5 padding for encryption. The `encrypt` and `decrypt` methods manage the encryption and decryption processes, respectively.

```java

    private static final String TRANSFORMATION = "AES/CBC/PKCS5Padding";

    private final SecretKey key;
    private final IvParameterSpec iv = new IvParameterSpec(
            new byte[]{25, -13, -25, -119, -42, 117, -118, -128, -101, 20, -103, -81, -48, -23, -54, -113});

    public EncryptionService(Encryption config) {
        this(config.getPassword(), config.getSalt());
    }

    EncryptionService(String password, String salt) {
        Objects.requireNonNull(password, "Encryption password is not set");
        Objects.requireNonNull(salt, "Encryption salt is not set");
        try {
            SecretKeyFactory secretKeyFactory = SecretKeyFactory.getInstance("PBKDF2WithHmacSHA256");
            KeySpec spec = new PBEKeySpec(password.toCharArray(), salt.getBytes(), 3000, 256);
            key = new SecretKeySpec(secretKeyFactory.generateSecret(spec).getEncoded(), "AES");
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/security/AccessService.java" line="17">

---

The `AccessService` class manages access control. It checks permissions for read and write operations on resources based on various conditions such as resource visibility, user roles, and specific access rules.

```java
public class AccessService {

    private final EncryptionService encryptionService;
    private final ShareService shareService;
    private final PublicationService publicationService;
    private final List<Rule> adminRules;

    public AccessService(EncryptionService encryptionService,
                         ShareService shareService,
                         PublicationService publicationService,
                         JsonObject settings) {
        this.encryptionService = encryptionService;
        this.shareService = shareService;
        this.publicationService = publicationService;
        this.adminRules = adminRules(settings);
    }

    public boolean hasReadAccess(ResourceDescription resource, ProxyContext context) {
        if (isAutoShared(resource, context)) {
            return true;
        }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/security/IdentityProvider.java" line="28">

---

The `IdentityProvider` class is responsible for identity verification. It handles JWT decoding, role extraction, and ensures that the JWTs are issued by a trusted issuer. It also manages a cache for JWKs to optimize the verification process.

```java
@Slf4j
public class IdentityProvider {

    // path to the claim of user roles in JWT
    private final String[] rolePath;

    private final JwkProvider jwkProvider;

    // in memory cache store results obtained from JWK provider
    private final ConcurrentHashMap<String, Future<JwkResult>> cache = new ConcurrentHashMap<>();

    // the name of the claim in JWT to extract user email
    private final String loggingKey;
    // random salt is used to digest user email
    private final String loggingSalt;

    private final MessageDigest sha256Digest;

    // the flag determines if user email should be obfuscated
    private final boolean obfuscateUserEmail;

```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYWktZGlhbC1jb3JlJTNBJTNBc3dpbW1pbw==" repo-name="ai-dial-core"><sup>Powered by [Swimm](/)</sup></SwmMeta>
