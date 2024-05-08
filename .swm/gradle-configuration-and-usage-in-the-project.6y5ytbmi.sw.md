---
title: Gradle Configuration and Usage in the Project
---
This document provides a detailed explanation of how Gradle is utilized within the ai-dial-core project, focusing on the configuration and execution steps defined in the Gradle scripts.

<SwmSnippet path="/gradlew" line="65">

---

# Gradle Wrapper Configuration

The Gradle wrapper script `gradlew` is crucial for ensuring that the correct Gradle version is used regardless of the local installation. It sets up the `APP_HOME` and adjusts the maximum file descriptors available, which is particularly important for performance on Unix-based systems.

```
# Attempt to set APP_HOME

# Resolve links: $0 may be a link
app_path=$0

# Need this for daisy-chained symlinks.
while
    APP_HOME=${app_path%"${app_path##*/}"}  # leaves a trailing /; empty if no leading path
    [ -h "$app_path" ]
do
    ls=$( ls -ld "$app_path" )
    link=${ls#*' -> '}
    case $link in             #(
      /*)   app_path=$link ;; #(
      *)    app_path=$APP_HOME$link ;;
    esac
done

# This is normally unused
# shellcheck disable=SC2034
APP_BASE_NAME=${0##*/}
```

---

</SwmSnippet>

<SwmSnippet path="/gradlew" line="118">

---

# Java Command Configuration

This section of the `gradlew` script determines the Java command to use based on the `JAVA_HOME` environment variable. It ensures that the correct Java executable is used to run Gradle, which is essential for compatibility and performance.

```
# Determine the Java command to use to start the JVM.
if [ -n "$JAVA_HOME" ] ; then
    if [ -x "$JAVA_HOME/jre/sh/java" ] ; then
        # IBM's JDK on AIX uses strange locations for the executables
        JAVACMD=$JAVA_HOME/jre/sh/java
    else
        JAVACMD=$JAVA_HOME/bin/java
    fi
    if [ ! -x "$JAVACMD" ] ; then
        die "ERROR: JAVA_HOME is set to an invalid directory: $JAVA_HOME

Please set the JAVA_HOME variable in your environment to match the
location of your Java installation."
    fi
else
    JAVACMD=java
    if ! command -v java >/dev/null 2>&1
    then
        die "ERROR: JAVA_HOME is not set and no 'java' command could be found in your PATH.

Please set the JAVA_HOME variable in your environment to match the
```

---

</SwmSnippet>

<SwmSnippet path="/build.gradle" line="1">

---

# Gradle Build Script

The `build.gradle` file configures plugins, dependencies, and the Java version for the project. It specifies the main class for the application and sets up the test environment, which includes configurations for logging test outcomes and using the JUnit platform.

```gradle
plugins {
    id "java"
    id 'checkstyle'
    id 'application'
    id 'io.freefair.lombok' version '8.0.1'
}

group = 'com.epam.aidial'
version = "0.9.0-rc"

java {
    toolchain {
        languageVersion = JavaLanguageVersion.of(17)
    }
}

checkstyle {
    configDirectory = file("$rootProject.projectDir/checkstyle")
}

repositories {
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYWktZGlhbC1jb3JlJTNBJTNBc3dpbW1pbw==" repo-name="ai-dial-core"><sup>Powered by [Swimm](/)</sup></SwmMeta>
