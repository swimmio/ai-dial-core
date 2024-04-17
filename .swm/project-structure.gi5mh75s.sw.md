---
title: Project structure
---
# Project structure

Welcome to the ai-dial-core repository, a HTTP Proxy that provides a unified API to various chat completion and embedding models, assistants, and applications. This repository is built using Java 17 and Eclipse Vert.x. It offers detailed instructions for building, running, and configuring the application. Additionally, it provides information on how to integrate Google Cloud Storage, Azure Blob Store, and Redis with the application. The following sections will guide you through the different components of the repository.

### Controller `(src/main/java/com/epam/aidial/core/controller)`

The Controller is an interface that standardizes HTTP controllers.

### Data `(src/main/java/com/epam/aidial/core/data)`

The Data class signifies a source, target, or review resource in the context of publication and storage operations.

### Limiter `(src/main/java/com/epam/aidial/core/limiter)`

The Limiter component is responsible for managing rate limits and controlling the number of tokens available for API requests.

### Service `(src/main/java/com/epam/aidial/core/service)`

The Service class provides functionality related to managing services and their configurations.

### Storage `(src/main/java/com/epam/aidial/core/storage)`

The BlobStorage class offers methods for storing, retrieving, and managing files in a blob storage system.

### Token `(src/main/java/com/epam/aidial/core/token)`

The Token class encapsulates the usage of tokens in the chat completion process, including information about completion tokens, prompt tokens, and total tokens.

### Util `(src/main/java/com/epam/aidial/core/util)`

The Util class represents a resource, containing information about the source, target, and review URLs of the resource.

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*


