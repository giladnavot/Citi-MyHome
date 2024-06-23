---
title: Building the Project with Docker
---
This document provides a detailed walkthrough of how Docker is used in the Citi-MyHome project, focusing on the Dockerfile and docker-compose.yml configuration files.

<SwmSnippet path="/Dockerfile" line="1">

---

## Dockerfile Configuration

The Dockerfile begins by specifying `azul/zulu-openjdk-alpine:8-jre` as the base image. This image includes the Java Runtime Environment (JRE) from Azul Systems, built on an Alpine Linux base for minimal size. The `ARG` instruction defines a build-time variable `JAR_FILE` which points to the location of the JAR file in the build context. The `COPY` instruction copies the JAR file into the Docker image as `app.jar`. The `EXPOSE` instruction informs Docker that the container listens on the specified network port at runtime, in this case, port 8080. The `ENTRYPOINT` instruction configures the container to run as an executable, specifying `java -jar /app.jar` as the command to run when the container starts.

```
FROM azul/zulu-openjdk-alpine:8-jre
ARG JAR_FILE=service/build/libs/*.jar
COPY ${JAR_FILE} app.jar
EXPOSE 8080
ENTRYPOINT ["java","-jar","/app.jar"]
```

---

</SwmSnippet>

<SwmSnippet path="/docker-compose.yml" line="1">

---

## Docker Compose Configuration

The docker-compose.yml file starts by specifying the version of the Docker Compose file format. It then defines a single service `myhome-service`. The `image` instruction specifies the image to start the container from, `jmprathab/myhome-service:${TAG:-latest}`. The `build` instruction tells Docker how to build the image, with the build context set to the current directory and the Dockerfile specified. The `container_name` instruction sets a custom name for the container. The `expose` and `ports` instructions expose port 8080 both inside the Docker network and on the host machine.

```yaml
version: '3.0'

services:
  myhome-service:
    image: jmprathab/myhome-service:${TAG:-latest}
    build:
      context: .
      dockerfile: Dockerfile
    container_name: "myhome-service"
    expose:
      - "8080"
    ports:
      - "8080:8080"
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBQ2l0aS1NeUhvbWUlM0ElM0FnaWxhZG5hdm90" repo-name="Citi-MyHome" doc-type="general-build-tool"><sup>Powered by [Swimm](/)</sup></SwmMeta>
