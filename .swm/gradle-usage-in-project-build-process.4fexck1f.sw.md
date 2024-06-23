---
title: Gradle Usage in Project Build Process
---
This document provides a detailed explanation of how Gradle is used in the Citi-MyHome project. It will cover the configuration files and their respective roles in the build process.

<SwmSnippet path="/settings.gradle" line="17">

---

# settings.gradle

This section of the `settings.gradle` file is responsible for managing the plugins used in the project. It specifies the versions of the Spring Boot, Spring Dependency Management, ResearchGate Release, and OpenAPI Generator plugins. It also sets the root project name to 'myhome-service' and includes 'integration-tests', 'service', and 'api' in the build.

```gradle
pluginManagement {
  plugins {
    id 'org.springframework.boot' version "${springBootVersion}"
    id 'io.spring.dependency-management' version "${springDependencyManagementVersion}"
    id 'net.researchgate.release' version "${researchgateReleaseVersion}"
    id "org.openapi.generator" version "${openApiVersion}"
  }
}

rootProject.name = 'myhome-service'
include 'integration-tests'
include 'service'
include 'api'

```

---

</SwmSnippet>

<SwmSnippet path="/build.gradle" line="17">

---

# build.gradle

The `build.gradle` file sets up the project's dependencies and configurations. It applies the 'java' and 'io.spring.dependency-management' plugins to all subprojects. It also sets up configurations for development and runtime classpaths.

```gradle
plugins {
  id 'io.spring.dependency-management'
  id 'java'
}

allprojects {
  repositories {
    mavenCentral()
  }
}

subprojects {
  group = 'com.prathab'
  version = '2.0.0'
  sourceCompatibility = '8'

  apply plugin: 'java'
  apply plugin: 'io.spring.dependency-management'

  configurations {
    developmentOnly
```

---

</SwmSnippet>

<SwmSnippet path="/gradlew" line="24">

---

# gradlew

The `gradlew` file is a Unix Shell script used to run the project with the Gradle Wrapper. It sets up the environment, determines the Java command to use, and executes the Gradle command.

```

# Attempt to set APP_HOME
# Resolve links: $0 may be a link
PRG="$0"
# Need this for relative symlinks.
while [ -h "$PRG" ] ; do
    ls=`ls -ld "$PRG"`
    link=`expr "$ls" : '.*-> \(.*\)$'`
    if expr "$link" : '/.*' > /dev/null; then
        PRG="$link"
    else
        PRG=`dirname "$PRG"`"/$link"
    fi
done
SAVED="`pwd`"
cd "`dirname \"$PRG\"`/" >/dev/null
APP_HOME="`pwd -P`"
cd "$SAVED" >/dev/null

APP_NAME="Gradle"
APP_BASE_NAME=`basename "$0"`
```

---

</SwmSnippet>

<SwmSnippet path="/gradlew.bat" line="24">

---

# gradlew.bat

The `gradlew.bat` file is a Windows Batch script used to run the project with the Gradle Wrapper. Similar to `gradlew`, it sets up the environment, determines the Java command to use, and executes the Gradle command.

```batchfile
@rem Set local scope for the variables with windows NT shell
if "%OS%"=="Windows_NT" setlocal

set DIRNAME=%~dp0
if "%DIRNAME%" == "" set DIRNAME=.
set APP_BASE_NAME=%~n0
set APP_HOME=%DIRNAME%

@rem Resolve any "." and ".." in APP_HOME to make it shorter.
for %%i in ("%APP_HOME%") do set APP_HOME=%%~fi

@rem Add default JVM options here. You can also use JAVA_OPTS and GRADLE_OPTS to pass JVM options to this script.
set DEFAULT_JVM_OPTS="-Xmx64m" "-Xms64m"

@rem Find java.exe
if defined JAVA_HOME goto findJavaFromJavaHome

set JAVA_EXE=java.exe
%JAVA_EXE% -version >NUL 2>&1
if "%ERRORLEVEL%" == "0" goto init

```

---

</SwmSnippet>

<SwmSnippet path="/gradle/wrapper/gradle-wrapper.properties" line="16">

---

# gradle-wrapper.properties

The `gradle-wrapper.properties` file contains properties for the Gradle Wrapper. It specifies the distribution base, path, and URL, as well as the zip store base and path.

```ini
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-6.3-bin.zip
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBQ2l0aS1NeUhvbWUlM0ElM0FnaWxhZG5hdm90" repo-name="Citi-MyHome" doc-type="general-build-tool"><sup>Powered by [Swimm](/)</sup></SwmMeta>
