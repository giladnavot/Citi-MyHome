---
title: User Management Overview
---
User Management in Citi-MyHome refers to the functionality that allows the system to handle operations related to users. This includes creating new users, listing all users, getting user details, resetting and requesting passwords, confirming email addresses, and resending email confirmations. These operations are mainly handled by the UserService interface and its implementation, UserSDJpaService. The UserController class uses these services to handle HTTP requests related to users. The UserRepository interface and its implementation provide methods for retrieving user data from the database.

<SwmSnippet path="/service/src/main/java/com/myhome/services/UserService.java" line="30">

---

## UserService Interface

The UserService interface defines the methods for user management. These include methods for creating a user, resending email confirmation, listing all users, getting user details, resetting passwords, and confirming email.

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

<SwmSnippet path="/service/src/main/java/com/myhome/controllers/UserController.java" line="50">

---

## UserController Class

The UserController class implements the UsersApi and uses the UserService to handle HTTP requests related to users. It includes methods for signing up a user, listing all users, getting user details, managing user passwords, listing all housemates of a user, confirming email, and resending confirmation email.

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

<SwmSnippet path="/service/src/main/java/com/myhome/repositories/UserRepository.java" line="36">

---

## UserRepository Interface

The UserRepository interface provides the `findByUserIdWithCommunities` method which is used to retrieve a user along with their communities from the database.

```java
  @Query("from User user where user.userId = :userId")
  @EntityGraph(value = "User.communities")
  Optional<User> findByUserIdWithCommunities(@Param("userId") String userId);
```

---

</SwmSnippet>

<SwmSnippet path="/service/src/main/java/com/myhome/controllers/dto/mapper/UserMapper.java" line="30">

---

## UserMapper Interface

The UserMapper interface provides the `userToUserDto` method which is used to convert a User object to a UserDto object. This is used in various places in the code to convert the data retrieved from the database into a format that can be used by the application.

```java
  UserDto userToUserDto(User user);
}
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBQ2l0aS1NeUhvbWUlM0ElM0FnaWxhZG5hdm90" repo-name="Citi-MyHome" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
