---
title: Introduction to User
---
In Citi-MyHome, a 'User' refers to an individual who interacts with the system. Users have unique identifiers and can be associated with multiple communities. They have various attributes such as email and password, and their details can be fetched, updated, and deleted. Users can also be assigned as administrators to communities.

<SwmSnippet path="/service/src/main/java/com/myhome/controllers/UserController.java" line="50">

---

## UserController

The UserController class provides the API endpoints for user-related operations. It uses the UserService to perform the actual business logic.

```java
@RestController
@Slf4j
@RequiredArgsConstructor
public class UserController implements UsersApi {

  private final UserService userService;
  private final UserApiMapper userApiMapper;
  private final HouseService houseService;
  private final HouseMemberMapper houseMemberMapper;

  @Override
  public ResponseEntity<CreateUserResponse> signUp(@Valid CreateUserRequest request) {
    log.trace("Received SignUp request");
    UserDto requestUserDto = userApiMapper.createUserRequestToUserDto(request);
    Optional<UserDto> createdUserDto = userService.createUser(requestUserDto);
    return createdUserDto
        .map(userDto -> {
          CreateUserResponse response = userApiMapper.userDtoToCreateUserResponse(userDto);
          return ResponseEntity.status(HttpStatus.CREATED).body(response);
        })
        .orElseGet(() -> ResponseEntity.status(HttpStatus.CONFLICT).build());
```

---

</SwmSnippet>

<SwmSnippet path="/service/src/main/java/com/myhome/services/UserService.java" line="30">

---

## UserService

The UserService interface defines the methods for user-related operations. These methods are implemented in the UserSDJpaService class.

```java
public interface UserService {
  Optional<UserDto> createUser(UserDto request);

  boolean resendEmailConfirm(String userId);

  Set<User> listAll();

  Set<User> listAll(Pageable pageable);

  Optional<UserDto> getUserDetails(String userId);

  boolean requestResetPassword(ForgotPasswordRequest forgotPasswordRequest);

  boolean resetPassword(ForgotPasswordRequest passwordResetRequest);

  Boolean confirmEmail(String userId, String emailConfirmToken);
}
```

---

</SwmSnippet>

<SwmSnippet path="/service/src/main/java/com/myhome/repositories/UserRepository.java" line="32">

---

## UserRepository

The UserRepository interface provides methods to interact with the database for user-related operations. It is used in the UserService implementation.

```java
  User findByEmail(String email);

  Optional<User> findByUserId(String userId);

  @Query("from User user where user.userId = :userId")
  @EntityGraph(value = "User.communities")
  Optional<User> findByUserIdWithCommunities(@Param("userId") String userId);

  @Query("from User user where user.userId = :userId")
  @EntityGraph(value = "User.userTokens")
  Optional<User> findByUserIdWithTokens(@Param("userId") String userId);

  @Query("from User user where user.email = :email")
  @EntityGraph(value = "User.userTokens")
  Optional<User> findByEmailWithTokens(@Param("email") String email);
```

---

</SwmSnippet>

<SwmSnippet path="/service/src/main/java/com/myhome/controllers/dto/mapper/UserMapper.java" line="28">

---

## UserMapper

The UserMapper interface provides methods to convert between UserDto and User entities. It is used in the UserService implementation.

```java
  User userDtoToUser(UserDto userDto);
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBQ2l0aS1NeUhvbWUlM0ElM0FnaWxhZG5hdm90" repo-name="Citi-MyHome" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
