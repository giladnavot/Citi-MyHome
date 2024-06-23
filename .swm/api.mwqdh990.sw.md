---
title: API
---
This document will cover the following aspects of the MyHome API:

1. Overview of the API and its offerings
2. Details on where to find the list of endpoints
3. Explanation of API documentation tools available in the repo

<SwmSnippet path="/api/src/main/resources/public/swagger/api.yaml" line="1">

---

# Overview of the API

The API is defined in the OpenAPI specification. It provides services related to various aspects of managing an apartment, including authentication, users, documents, communities, amenities, houses, payments, and members.

```yaml
openapi: 3.0.2
servers:
  - url: http://localhost:8080/
info:
  description: This is a OpenApi specification for MyHome backend service.
  version: 2.0.0
  title: Swagger MyHome - OpenAPI 3.0
```

---

</SwmSnippet>

<SwmSnippet path="/api/src/main/resources/public/swagger/api.yaml" line="17">

---

# List of Endpoints

The list of all available endpoints can be found in the OpenAPI specification. Each endpoint is associated with a specific tag, such as 'Authentication', 'Users', 'Documents', etc.

```yaml
paths:
  /auth/login:
    post:
      tags:
        - Authentication
      description: Login user to system
      operationId: login
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/LoginRequest'
          application/xml:
            schema:
              $ref: '#/components/schemas/LoginRequest'
      responses:
        '200':
          description: Login successful

  /users/password:
```

---

</SwmSnippet>

<SwmSnippet path="/api/src/main/resources/public/swagger/api.yaml" line="1">

---

# Swagger

The API is available via Swagger. The specification path is defined in the `api.yaml` file.

```yaml
openapi: 3.0.2
servers:
  - url: http://localhost:8080/
info:
  description: This is a OpenApi specification for MyHome backend service.
  version: 2.0.0
  title: Swagger MyHome - OpenAPI 3.0
tags:
  - name: Authentication
  - name: Users
  - name: Documents
  - name: Communities
  - name: Amenities
  - name: Houses
  - name: Payments
  - name: Members
paths:
  /auth/login:
    post:
      tags:
        - Authentication
```

---

</SwmSnippet>

<SwmSnippet path="/postman/MyHome.postman_collection.json" line="1">

---

# Postman

The API is also available via Postman. The collection is defined in the `MyHome.postman_collection.json` file.

```json
{
	"info": {
		"_postman_id": "f6220240-de74-4e4f-9916-6aa1cf6ad5d8",
		"name": "MyHome",
		"description": "# MyHome API Guide\r\n\r\n## Usage\r\n\r\nTo use any API except **Create User and Login User**, you'll need to obtain Authentication Token.\r\n\r\nTo get Authentication Token.\r\n\r\n1. First Create New User with **Create User API**.\r\n2. Login with New user with **New User API**. Login will obtain New Token for user.\r\n\r\nNow you can use other APIs.\r\n\r\n\r\n## Variables\r\n\r\nAll APIs are configured with Postman variables to keep consistensy for static and dynamic values.\r\n\r\n1. **ENV_URL**: This variable hold value for Enviroment URL. e.g. 127.0.0.1:8080 (Default to local environment, with 8080 port), or example.com.\r\n2. **AUTH_TOKEN**: This variable hold authentication token obtained by Login User API, which is used in request header. This is stored each time you use Login User API.\r\n3. **COMMUNITY_ID**: This variable hold default community id created for testing Community related APIs.\r\n4. **HOUSE_ID**: This variable hold default house id created for testing House related APIs.\r\n5. **USER_ID**: This variable hold userId obtained by Login User API. This is stored each tiem you use Login User API.\r\n6. **ADMIN_ID**: This variable hold default admin id created for testing related APIs.\r\n7. **MEMBER_ID**: This variable hold default member id created for testing related APIs.\r\n\r\n\r\nYou can configure your own values for these either by editing in *MyHome > ... (View more actions) > Edit > Variables Tab* or you can create your environment and add variable there. [How to create Environment in Postman] (https://learning.postman.com/docs/postman/variables-and-environments/managing-environments/#creating-environments)",
		"schema": "https://schema.getpostman.com/json/collection/v2.1.0/collection.json"
	},
	"item": [
		{
			"name": "authentication",
			"item": [
				{
					"name": "requestResetPassword",
					"item": [
						{
							"name": "Request reset password - Forgot",
							"protocolProfileBehavior": {
								"disabledSystemHeaders": {}
							},
							"request": {
								"method": "POST",
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBQ2l0aS1NeUhvbWUlM0ElM0FnaWxhZG5hdm90" repo-name="Citi-MyHome" doc-type="api"><sup>Powered by [Swimm](/)</sup></SwmMeta>
