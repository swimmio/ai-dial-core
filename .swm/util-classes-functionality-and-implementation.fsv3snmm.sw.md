---
title: Util Classes Functionality and Implementation
---
This document will cover the functionalities and implementations of Util classes in the ai-dial-core project:

# Overview of Util Classes

Util classes in the ai-dial-core project are utility classes marked with `@UtilityClass` annotation from Lombok, which makes all methods static and prevents instantiation. These classes provide a variety of static methods that are used throughout the project to perform common tasks such as URL encoding/decoding, resource checking, data compression, and HTTP exception handling.

<SwmSnippet path="/src/main/java/com/epam/aidial/core/util/UrlUtil.java" line="9">

---

# Implementation Details

The `UrlUtil` class provides methods for encoding and decoding URLs. `encodePath` method encodes a path using URI encoding rules, while `decodePath` method decodes it ensuring that no illegal characters are present.

```java
public class UrlUtil {

    public String encodePath(String path) {
        try {
            URI uri = new URI(null, null, path, null);
            return uri.toASCIIString();
        } catch (URISyntaxException e) {
            throw new RuntimeException(e);
        }
    }

    public String decodePath(String path) {
        try {
            URI uri = new URI(path);
            if (uri.getRawFragment() != null || uri.getRawQuery() != null) {
                throw new IllegalArgumentException("Wrong path provided " + path);
            }
            return uri.getPath();
        } catch (URISyntaxException e) {
            throw new RuntimeException(e);
        }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/util/ResourceUtil.java" line="9">

---

# Implementation Details

The `ResourceUtil` class contains methods to check the existence of resources. The `hasResource` method determines if a resource exists based on its type, utilizing services and storage mechanisms.

```java
public class ResourceUtil {

    public static boolean hasResource(ResourceDescription resource, ResourceService resourceService, BlobStorage storage) {
        return switch (resource.getType()) {
            case FILE -> storage.exists(resource.getAbsoluteFilePath());
            case CONVERSATION, PROMPT -> resourceService.hasResource(resource);
            default -> throw new IllegalArgumentException("Unsupported resource type " + resource.getType());
        };
    }
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/util/Base58.java" line="46">

---

# Implementation Details

The `Base58` class provides methods for encoding and decoding data using the Base58 encoding scheme, which is commonly used in cryptocurrencies like Bitcoin to produce readable and non-confusing codes.

```java
public class Base58 {
    public static final char[] ALPHABET = "123456789ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz".toCharArray();
    private static final char ENCODED_ZERO = ALPHABET[0];
    private static final int[] INDEXES = new int[128];

    static {
        Arrays.fill(INDEXES, -1);
        for (int i = 0; i < ALPHABET.length; i++) {
            INDEXES[ALPHABET[i]] = i;
        }
    }

    /**
     * Encodes the given bytes as a base58 string (no checksum is appended).
     *
     * @param input the bytes to encode
     * @return the base58-encoded string
     */
    public static String encode(byte[] input) {
        if (input.length == 0) {
            return "";
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/util/Compression.java" line="16">

---

# Implementation Details

The `Compression` class offers methods for data compression and decompression specifically with GZIP format. It handles both byte arrays and input streams, providing flexibility depending on the data source.

```java
public class Compression {

    @SneakyThrows
    public byte[] compress(String type, byte[] data) {
        if (!type.equals("gzip")) {
            throw new IllegalArgumentException("Unsupported compression: " + type);
        }

        ByteArrayOutputStream output = new ByteArrayOutputStream();
        try (GZIPOutputStream stream = new GZIPOutputStream(output)) {
            stream.write(data);
        }
        return output.toByteArray();
    }

    @SneakyThrows
    public byte[] decompress(String type, InputStream input) {
        if (!type.equals("gzip")) {
            throw new IllegalArgumentException("Unsupported compression: " + type);
        }

```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/util/HttpException.java" line="6">

---

# Implementation Details

The `HttpException` class is a custom exception class that encapsulates HTTP status codes along with an error message, allowing for more descriptive error handling in HTTP operations.

```java
public class HttpException extends RuntimeException {
    private final HttpStatus status;

    public HttpException(HttpStatus status, String message) {
        super(message);
        this.status = status;
    }
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYWktZGlhbC1jb3JlJTNBJTNBc3dpbW1pbw==" repo-name="ai-dial-core"><sup>Powered by [Swimm](/)</sup></SwmMeta>
