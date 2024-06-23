---
title: Gradle Configuration in Service Module
---
This document provides a detailed walkthrough of how Gradle is used in the service module of our project. It focuses on the configuration and dependencies defined in the `build.gradle` file.

<SwmSnippet path="/service/build.gradle" line="17">

---

# Plugins Configuration

The `plugins` block is where we define the plugins that are required for our project. Here, we are using the 'org.springframework.boot', 'jacoco', and 'net.researchgate.release' plugins.

```gradle
plugins {
  id 'org.springframework.boot'
  id 'jacoco'
  id 'net.researchgate.release'
}
```

---

</SwmSnippet>

<SwmSnippet path="/service/build.gradle" line="23">

---

# Dependencies Configuration

The `dependencies` block is where we define the libraries that our project depends on. This includes the Spring Boot starters for web, actuator, test, security, and data JPA, as well as other libraries like H2, Devtools, Json web token, Mapstruct, Lombok, and mail.

```gradle
dependencies {

  implementation project(":api")

  // Spring web
  implementation 'org.springframework.boot:spring-boot-starter-web'

  // Spring actuator
  implementation 'org.springframework.boot:spring-boot-starter-actuator'

  // Spring test
  testImplementation('org.springframework.boot:spring-boot-starter-test') {
    exclude group: 'org.junit.vintage', module: 'junit-vintage-engine'
  }

  // Spring security
  implementation 'org.springframework.boot:spring-boot-starter-security'
  testImplementation 'org.springframework.security:spring-security-test'

  // Spring JPA
  implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
```

---

</SwmSnippet>

<SwmSnippet path="/service/build.gradle" line="77">

---

# Jar Configuration

The `jar` block is where we configure the creation of the regular JAR file. This is necessary for dependent projects like integration-test to properly use classes from this project.

```gradle
jar {
  archiveBaseName = 'myhome-service'
  enabled = true
}
```

---

</SwmSnippet>

<SwmSnippet path="/service/build.gradle" line="82">

---

# BootJar Configuration

The `bootJar` block is where we configure the creation of the bootable JAR file. This includes setting the base name, enabling the creation, setting the main class name, and defining the manifest attributes.

```gradle
bootJar {
  archiveBaseName = 'myhome-service-boot'
  enabled = true

  mainClassName = 'com.myhome.MyHomeServiceApplication'
  manifest {
    attributes(
            'Implementation-Title': 'myhome-service',
            'Implementation-Version': archiveVersion
    )
  }
}
```

---

</SwmSnippet>

<SwmSnippet path="/service/build.gradle" line="95">

---

# CompileJava Configuration

The `compileJava` block is where we configure the Java compilation. This includes setting the annotation processor path and defining the compiler arguments.

```gradle
compileJava {
  // Mapstruct options
  options.annotationProcessorPath = configurations.annotationProcessor
  options.compilerArgs << "-Amapstruct.defaultComponentModel=spring"
  options.compilerArgs << "-Amapstruct.unmappedTargetPolicy=IGNORE"
}
```

---

</SwmSnippet>

<SwmSnippet path="/service/build.gradle" line="102">

---

# Test Configuration

The `test` block is where we configure the testing. Here, we specify that we want to use the JUnit Platform.

```gradle
test {
  useJUnitPlatform()
}
```

---

</SwmSnippet>

<SwmSnippet path="/service/build.gradle" line="106">

---

# Jacoco Configuration

The `jacoco` block is where we configure the Jacoco test report. This includes setting the dependencies, enabling the reports, setting the destination for the HTML report, and excluding certain directories from the report.

```gradle
// Jacoco
test.finalizedBy jacocoTestReport

jacocoTestReport {
  dependsOn {
    test
  }
  reports {
    xml.enabled true
    csv.enabled false
    html.destination file("${buildDir}/jacoco/jacocoHtml")
  }

  // Keep this until mapstruct comes up with a better solution
  // https://github.com/mapstruct/mapstruct/issues/1528
  afterEvaluate {
    classDirectories.setFrom(files(classDirectories.files.collect {
      fileTree(dir: it, exclude: ['**/mapper/**', '**/mapper'])
    }))
  }
}
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBQ2l0aS1NeUhvbWUlM0ElM0FnaWxhZG5hdm90" repo-name="Citi-MyHome" doc-type="general-build-tool"><sup>Powered by [Swimm](/)</sup></SwmMeta>
