---
title: Exploring the Security Token
---
A Security Token in the Citi-MyHome project is a unique identifier generated for specific security purposes. It is used for actions such as email confirmation and password reset. The SecurityTokenService interface provides methods for creating and using these tokens.

The SecurityTokenService is implemented by the SecurityTokenSDJpaService class. This class uses the SecurityTokenRepository to persist the tokens in the database.

The createSecurityToken method generates a new SecurityToken. It assigns a unique UUID to the token, sets the creation and expiry dates, and associates the token with a user. The token is then saved in the repository.

The createEmailConfirmToken and createPasswordResetToken methods use the createSecurityToken method to generate tokens for email confirmation and password reset respectively.

The useToken method marks a token as used and updates it in the repository.

<SwmSnippet path="/service/src/main/java/com/myhome/services/SecurityTokenService.java" line="6">

---

# SecurityTokenService

The SecurityTokenService interface provides methods to create and use Security Tokens. It has methods like `createEmailConfirmToken`, `createPasswordResetToken`, and `useToken`.

```java
public interface SecurityTokenService {

  SecurityToken createEmailConfirmToken(User owner);

  SecurityToken createPasswordResetToken(User owner);

  SecurityToken useToken(SecurityToken token);
}
```

---

</SwmSnippet>

<SwmSnippet path="/service/src/main/java/com/myhome/services/springdatajpa/SecurityTokenSDJpaService.java" line="16">

---

# SecurityTokenSDJpaService

The SecurityTokenSDJpaService class implements the SecurityTokenService interface. It provides the actual implementation of the methods declared in the interface. It uses the `SecurityTokenRepository` to save and retrieve the tokens.

```java
@Service
@RequiredArgsConstructor
public class SecurityTokenSDJpaService implements SecurityTokenService {

  private final SecurityTokenRepository securityTokenRepository;

  @Value("${tokens.reset.expiration}")
  private Duration passResetTokenTime;
  @Value("${tokens.email.expiration}")
  private Duration emailConfirmTokenTime;

  private SecurityToken createSecurityToken(SecurityTokenType tokenType, Duration liveTimeSeconds, User tokenOwner) {
    String token = UUID.randomUUID().toString();
    LocalDate creationDate = LocalDate.now();
    LocalDate expiryDate = getDateAfterDays(LocalDate.now(), liveTimeSeconds);
    SecurityToken newSecurityToken = new SecurityToken(tokenType, token, creationDate, expiryDate, false, null);
    newSecurityToken.setTokenOwner(tokenOwner);
    newSecurityToken = securityTokenRepository.save(newSecurityToken);
    return newSecurityToken;
  }

```

---

</SwmSnippet>

<SwmSnippet path="/service/src/main/java/com/myhome/services/springdatajpa/SecurityTokenSDJpaService.java" line="27">

---

# Creating Security Tokens

The `createSecurityToken` method is used to create a new Security Token. It generates a random UUID for the token, sets the creation and expiry dates, and saves the token using the `SecurityTokenRepository`.

```java
  private SecurityToken createSecurityToken(SecurityTokenType tokenType, Duration liveTimeSeconds, User tokenOwner) {
    String token = UUID.randomUUID().toString();
    LocalDate creationDate = LocalDate.now();
    LocalDate expiryDate = getDateAfterDays(LocalDate.now(), liveTimeSeconds);
    SecurityToken newSecurityToken = new SecurityToken(tokenType, token, creationDate, expiryDate, false, null);
    newSecurityToken.setTokenOwner(tokenOwner);
    newSecurityToken = securityTokenRepository.save(newSecurityToken);
    return newSecurityToken;
  }
```

---

</SwmSnippet>

<SwmSnippet path="/service/src/main/java/com/myhome/services/springdatajpa/SecurityTokenSDJpaService.java" line="48">

---

# Using Security Tokens

The `useToken` method is used to mark a Security Token as used. It sets the `used` field of the token to `true` and saves the updated token using the `SecurityTokenRepository`.

```java
  public SecurityToken useToken(SecurityToken token) {
    token.setUsed(true);
    token = securityTokenRepository.save(token);
    return token;
  }
```

---

</SwmSnippet>

# Security Token Functions

The Security Token functions

<SwmSnippet path="/service/src/main/java/com/myhome/services/springdatajpa/SecurityTokenSDJpaService.java" line="27">

---

## createSecurityToken

The `createSecurityToken` function is a private method used to create a new security token. It takes in a token type, a duration for the token's lifetime, and a user who will own the token. It generates a random UUID for the token, sets the creation and expiry dates, and saves the new token in the repository.

```java
  private SecurityToken createSecurityToken(SecurityTokenType tokenType, Duration liveTimeSeconds, User tokenOwner) {
    String token = UUID.randomUUID().toString();
    LocalDate creationDate = LocalDate.now();
    LocalDate expiryDate = getDateAfterDays(LocalDate.now(), liveTimeSeconds);
    SecurityToken newSecurityToken = new SecurityToken(tokenType, token, creationDate, expiryDate, false, null);
    newSecurityToken.setTokenOwner(tokenOwner);
    newSecurityToken = securityTokenRepository.save(newSecurityToken);
    return newSecurityToken;
  }
```

---

</SwmSnippet>

<SwmSnippet path="/service/src/main/java/com/myhome/services/springdatajpa/SecurityTokenSDJpaService.java" line="38">

---

## createEmailConfirmToken

The `createEmailConfirmToken` function is a public method that creates an email confirmation token for a user. It calls the `createSecurityToken` function with the EMAIL_CONFIRM token type and the email confirmation token time.

```java
  public SecurityToken createEmailConfirmToken(User tokenOwner) {
    return createSecurityToken(SecurityTokenType.EMAIL_CONFIRM, emailConfirmTokenTime, tokenOwner);
  }
```

---

</SwmSnippet>

<SwmSnippet path="/service/src/main/java/com/myhome/services/springdatajpa/SecurityTokenSDJpaService.java" line="43">

---

## createPasswordResetToken

The `createPasswordResetToken` function is a public method that creates a password reset token for a user. It calls the `createSecurityToken` function with the RESET token type and the password reset token time.

```java
  public SecurityToken createPasswordResetToken(User tokenOwner) {
    return createSecurityToken(SecurityTokenType.RESET, passResetTokenTime, tokenOwner);
  }
```

---

</SwmSnippet>

<SwmSnippet path="/service/src/main/java/com/myhome/services/springdatajpa/SecurityTokenSDJpaService.java" line="48">

---

## useToken

The `useToken` function is a public method that marks a token as used and saves the updated token in the repository.

```java
  public SecurityToken useToken(SecurityToken token) {
    token.setUsed(true);
    token = securityTokenRepository.save(token);
    return token;
  }
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBQ2l0aS1NeUhvbWUlM0ElM0FnaWxhZG5hdm90" repo-name="Citi-MyHome" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
