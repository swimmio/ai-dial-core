---
title: Configuring Docker for the ai-dial-core Project
---
This document details the steps involved in configuring Docker for the ai-dial-core project, focusing on the Dockerfile setup.

<SwmSnippet path="/Dockerfile" line="1">

---

# Initial Setup and Dependency Caching

The Dockerfile begins by setting up a Gradle environment on an Alpine Linux with JDK 17, creating a working directory, and copying the Gradle configuration files. It then performs a preliminary build to cache dependencies, enhancing subsequent build speeds.

```
FROM gradle:8.2.0-jdk17-alpine as cache

WORKDIR /home/gradle/src
ENV GRADLE_USER_HOME /cache
COPY build.gradle settings.gradle ./
# just pull dependencies for cache
RUN gradle --no-daemon build --stacktrace
```

---

</SwmSnippet>

<SwmSnippet path="/Dockerfile" line="9">

---

# Building the Application

In the next stage, the Dockerfile uses another Gradle image to copy the cached dependencies and the application source code. It then executes the build command excluding tests and compressions, and extracts the build artifacts to a designated directory.

```
FROM gradle:8.2.0-jdk17-alpine as builder

COPY --from=cache /cache /home/gradle/.gradle
COPY --chown=gradle:gradle . /home/gradle/src

WORKDIR /home/gradle/src
RUN gradle --no-daemon build --stacktrace -PdisableCompression=true -x test
RUN mkdir /build && tar -xf /home/gradle/src/build/distributions/aidial-core*.tar --strip-components=1 -C /build
```

---

</SwmSnippet>

<SwmSnippet path="/Dockerfile" line="18">

---

# Preparing the Runtime Environment

The final image is based on Eclipse Temurin JDK Alpine. It includes security fixes, adds necessary runtime utilities, and sets environment variables for OpenTelemetry and local storage paths. It also configures user permissions for better security.

```
FROM eclipse-temurin:17-jdk-alpine

# fix CVE-2023-5363
# TODO remove the fix once a new version is released
RUN apk update && apk upgrade --no-cache libcrypto3 libssl3
# fix CVE-2023-52425
RUN apk upgrade --no-cache libexpat
RUN apk add --no-cache su-exec

ENV OTEL_TRACES_EXPORTER="none"
ENV OTEL_METRICS_EXPORTER="none"
ENV OTEL_LOGS_EXPORTER="none"

# Local storage dir configured in the default aidial.settings.json
ENV STORAGE_DIR /app/data
ENV LOG_DIR /app/log

WORKDIR /app

RUN adduser -u 1001 --disabled-password --gecos "" appuser

```

---

</SwmSnippet>

<SwmSnippet path="/Dockerfile" line="37">

---

# Final Configuration and Health Check

The application is set up under a non-root user for security. The Docker entrypoint script is added and made executable. A health check is configured to ensure the application's health post-deployment. Finally, necessary ports are exposed and the entrypoint is defined to start the application.

```
RUN adduser -u 1001 --disabled-password --gecos "" appuser

COPY --from=builder --chown=appuser:appuser /build/ .
RUN chown -R appuser:appuser /app

COPY --chown=appuser:appuser docker-entrypoint.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/docker-entrypoint.sh

HEALTHCHECK --start-period=30s --interval=1m --timeout=3s \
  CMD wget --no-verbose --spider --tries=1 http://localhost:8080/health || exit 1

EXPOSE 8080 9464

RUN mkdir -p "$LOG_DIR" && \
    chown -R appuser:appuser "$LOG_DIR" && \
    mkdir -p "$STORAGE_DIR" && \
    chown -R appuser:appuser "$STORAGE_DIR"

ENTRYPOINT ["docker-entrypoint.sh"]
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYWktZGlhbC1jb3JlJTNBJTNBc3dpbW1pbw==" repo-name="ai-dial-core"><sup>Powered by [Swimm](/)</sup></SwmMeta>
