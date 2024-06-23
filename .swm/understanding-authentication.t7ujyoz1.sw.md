---
title: Understanding Authentication
---
Authentication in the Citi-MyHome project is handled by the <SwmToken path="/service/src/main/java/com/myhome/services/AuthenticationService.java" pos="6:4:4" line-data="public interface AuthenticationService {">`AuthenticationService`</SwmToken> interface, which defines the <SwmToken path="/service/src/main/java/com/myhome/services/AuthenticationService.java" pos="7:3:3" line-data="  AuthenticationData login(LoginRequest loginRequest);">`login`</SwmToken> method. This method is implemented in the <SwmToken path="/service/src/main/java/com/myhome/services/springdatajpa/AuthenticationSDJpaService.java" pos="20:4:4" line-data="public class AuthenticationSDJpaService implements AuthenticationService {">`AuthenticationSDJpaService`</SwmToken> class, where it checks the user's credentials and generates a JWT token if the credentials are valid. The <SwmToken path="/service/src/main/java/com/myhome/controllers/AuthenticationController.java" pos="15:4:4" line-data="public class AuthenticationController implements AuthenticationApi {">`AuthenticationController`</SwmToken>`icationService` to handle login requests from users. If the login is successful, it returns a response with the user's ID and JWT token in the headers. If the login fails, an <SwmToken path="/service/src/main/java/com/myhome/controllers/exceptions/AuthenticationException.java" pos="9:4:4" line-data="public class AuthenticationException extends RuntimeException {">`AuthenticationException`</SwmToken> is thrown.

<SwmSnippet path="/service/src/main/java/com/myhome/services/AuthenticationService.java" line="6">

---

# <SwmToken path="/service/src/main/java/com/myhome/services/AuthenticationService.java" pos="6:4:4" line-data="public interface AuthenticationService {">`AuthenticationService`</SwmToken> Interface

The <SwmToken path="/service/src/main/java/com/myhome/controllers/AuthenticationController.java" pos="17:5:5" line-data="  private final AuthenticationService authenticationService;">`AuthenticationService`</SwmToken> interface defines the contract for the authentication service. It declares a login method that takes a LoginRequest object and returns an <SwmToken path="/service/src/main/java/com/myhome/services/AuthenticationService.java" pos="7:1:1" line-data="  AuthenticationData login(LoginRequest loginRequest);">`AuthenticationData`</SwmToken> object.

```java
public interface AuthenticationService {
  AuthenticationData login(LoginRequest loginRequest);
}
```

---

</SwmSnippet>

<SwmSnippet path="/service/src/main/java/com/myhome/services/springdatajpa/AuthenticationSDJpaService.java" line="19">

---

# <SwmToken path="/service/src/main/java/com/myhome/services/springdatajpa/AuthenticationSDJpaService.java" pos="20:4:4" line-data="public class AuthenticationSDJpaService implements AuthenticationService {">`AuthenticationSDJpaService`</SwmToken> Class

The <SwmToken path="/service/src/main/java/com/myhome/services/springdatajpa/AuthenticationSDJpaService.java" pos="20:4:4" line-data="public class AuthenticationSDJpaService implements AuthenticationService {">`AuthenticationSDJpaService`</SwmToken> class implements the <SwmToken path="/service/src/main/java/com/myhome/controllers/AuthenticationController.java" pos="17:5:5" line-data="  private final AuthenticationService authenticationService;">`AuthenticationService`</SwmToken> interface. It provides the actual implementation of the login method. This method authenticates the user and returns an <SwmToken path="/service/src/main/java/com/myhome/services/AuthenticationService.java" pos="7:1:1" line-data="  AuthenticationData login(LoginRequest loginRequest);">`AuthenticationData`</SwmToken> object containing the user's authentication data.

```java
@Service
public class AuthenticationSDJpaService implements AuthenticationService {

  private final Duration tokenExpirationTime;
  private final String tokenSecret;

  private final UserSDJpaService userSDJpaService;
  private final AppJwtEncoderDecoder appJwtEncoderDecoder;
  private final PasswordEncoder passwordEncoder;

  public AuthenticationSDJpaService(@Value("${token.expiration_time}") Duration tokenExpirationTime,
      @Value("${token.secret}") String tokenSecret,
      UserSDJpaService userSDJpaService,
      AppJwtEncoderDecoder appJwtEncoderDecoder,
      PasswordEncoder passwordEncoder) {
    this.tokenExpirationTime = tokenExpirationTime;
    this.tokenSecret = tokenSecret;
    this.userSDJpaService = userSDJpaService;
    this.appJwtEncoderDecoder = appJwtEncoderDecoder;
    this.passwordEncoder = passwordEncoder;
  }
```

---

</SwmSnippet>

<SwmSnippet path="/service/src/main/java/com/myhome/controllers/AuthenticationController.java" line="13">

---

# <SwmToken path="/service/src/main/java/com/myhome/controllers/AuthenticationController.java" pos="15:4:4" line-data="public class AuthenticationController implements AuthenticationApi {">`AuthenticationController`</SwmToken> Class

The AuthenticationController class uses the AuthenticationService to authenticate the user. It calls the login method of the AuthenticationService and generates a response with the user's authentication data.

```java
@RequiredArgsConstructor
@RestController
public class AuthenticationController implements AuthenticationApi {

  private final AuthenticationService authenticationService;

  @Override
  public ResponseEntity<Void> login(@Valid LoginRequest loginRequest) {
    final AuthenticationData authenticationData = authenticationService.login(loginRequest);
    return ResponseEntity.ok()
        .headers(createLoginHeaders(authenticationData))
        .build();
  }

  private HttpHeaders createLoginHeaders(AuthenticationData authenticationData) {
    final HttpHeaders httpHeaders = new HttpHeaders();
    httpHeaders.add("userId", authenticationData.getUserId());
    httpHeaders.add("token", authenticationData.getJwtToken());
    return httpHeaders;
  }
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBQ2l0aS1NeUhvbWUlM0ElM0FnaWxhZG5hdm90" repo-name="Citi-MyHome"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>
