---
title: How does multipart file upload work?
---
This document will cover the process of multipart file upload in the ai-dial-core project. We'll cover:

1. Reading data from an InputStream
2. Handling data and initiating multipart upload
3. Building metadata for the blob
4. Creating the appropriate credential provider.

<SwmSnippet path="/src/main/java/com/epam/aidial/core/storage/InputStreamReader.java" line="40">

---

# Reading data from an InputStream

The `InputStreamReader` constructor sets up a handler for the data read from the InputStream. It also sets up a `drainHandler` which is triggered when the buffer is full and needs to be drained.

```java
    public InputStreamReader(Vertx vertx, InputStream in, int bufferSize) {
        this.vertx = vertx;
        this.in = in;
        this.queue = new InboundBuffer<>(vertx.getOrCreateContext(), 32);
        this.bufferSize = bufferSize;
        queue.handler(buff -> {
            if (buff.length() > 0) {
                handleData(buff);
            } else {
                handleEnd();
            }
        });
        queue.drainHandler(v -> readDataFromStream());
        queue.pause();
        readDataFromStream();
    }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/storage/BlobWriteStream.java" line="144">

---

# Handling data and initiating multipart upload

The `drainHandler` in `BlobWriteStream` is responsible for handling the data read from the InputStream. It initiates a multipart upload if it hasn't been started already.

```java
    public WriteStream<Buffer> drainHandler(Handler<Void> handler) {
        vertx.executeBlocking(() -> {
            synchronized (BlobWriteStream.this) {
                try {
                    if (mpu == null) {
                        mpu = storage.initMultipartUpload(resource.getAbsoluteFilePath(), contentType);
                    }
                    MultipartPart part = storage.storeMultipartPart(mpu, ++chunkNumber, chunkBuffer.slice(0, position));
                    parts.add(part);
                    position = 0;
                    isBufferFull = false;
                } catch (Throwable ex) {
                    exception = ex;
                } finally {
                    if (handler != null) {
                        handler.handle(null);
                    }
                }
            }
            return null;
        });
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/storage/BlobStorage.java" line="84">

---

# Building metadata for the blob

The `initMultipartUpload` method in `BlobStorage` builds the metadata for the blob to be uploaded. It calls `buildBlobMetadata` which in turn calls `buildContentMetadata` to set the content type of the blob.

```java
    @SuppressWarnings("UnstableApiUsage") // multipart upload uses beta API
    public MultipartUpload initMultipartUpload(String absoluteFilePath, String contentType) {
        String storageLocation = getStorageLocation(absoluteFilePath);
        BlobMetadata metadata = buildBlobMetadata(storageLocation, contentType, bucketName);
        return blobStore.initiateMultipartUpload(bucketName, metadata, PutOptions.NONE);
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/java/com/epam/aidial/core/storage/credential/CredentialProviderFactory.java" line="8">

---

# Creating the appropriate credential provider

The `create` method in `CredentialProviderFactory` creates the appropriate credential provider based on the storage provider name.

```java
    public static CredentialProvider create(String providerName, String identity, String credential) {
        StorageProvider provider = StorageProvider.from(providerName);
        return switch (provider) {
            case S3 -> new DefaultCredentialProvider(identity, credential);
            case AZURE_BLOB -> new AzureCredentialProvider(identity, credential);
            case GOOGLE_CLOUD_STORAGE -> new GcpCredentialProvider(identity, credential);
            case FILESYSTEM -> new DefaultCredentialProvider("identity", "credential");
            case AWS_S3 -> new AwsCredentialProvider(identity, credential);
        };
    }
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*


