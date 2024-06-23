---
title: Overview of Community
---
In the Citi-MyHome project, 'Community' refers to a key domain concept that represents a group of houses or apartments. It is implemented as a class in the codebase, with various operations that can be performed such as creating a new community, listing all communities, getting details of a specific community, and managing community administrators and houses.

The 'CommunityController' class provides the REST API endpoints for these operations, while the 'CommunityService' interface defines the service layer methods for these operations. The actual implementation of these methods is provided in the 'CommunitySDJpaService' class.

The 'CommunityRepository' interface provides the data access methods for the 'Community' entity, such as finding a community by its ID along with its houses. These methods are used in the service layer to interact with the database.

<SwmSnippet path="/service/src/main/java/com/myhome/controllers/CommunityController.java" line="56">

---

# CommunityController

The CommunityController is a REST controller that handles HTTP requests related to communities. It uses the CommunityService to perform the actual operations.

```java
@RequiredArgsConstructor
@RestController
@Slf4j
public class CommunityController implements CommunitiesApi {
  private final CommunityService communityService;
  private final CommunityApiMapper communityApiMapper;

  @Override
  public ResponseEntity<CreateCommunityResponse> createCommunity(@Valid @RequestBody
      CreateCommunityRequest request) {
    log.trace("Received create community request");
    CommunityDto requestCommunityDto =
        communityApiMapper.createCommunityRequestToCommunityDto(request);
    Community createdCommunity = communityService.createCommunity(requestCommunityDto);
    CreateCommunityResponse createdCommunityResponse =
        communityApiMapper.communityToCreateCommunityResponse(createdCommunity);
    return ResponseEntity.status(HttpStatus.CREATED).body(createdCommunityResponse);
  }

  @Override
  public ResponseEntity<GetCommunityDetailsResponse> listAllCommunity(
```

---

</SwmSnippet>

<SwmSnippet path="/service/src/main/java/com/myhome/services/CommunityService.java" line="28">

---

# CommunityService

The CommunityService is an interface that defines the operations that can be performed on communities. These operations include creating a community, listing all communities, and adding houses or admins to a community.

```java
public interface CommunityService {
  Community createCommunity(CommunityDto communityDto);

  Set<Community> listAll();

  Set<Community> listAll(Pageable pageable);

  Optional<Community> getCommunityDetailsById(String communityId);

  Optional<List<CommunityHouse>> findCommunityHousesById(String communityId, Pageable pageable);

  Optional<List<User>> findCommunityAdminsById(String communityId, Pageable pageable);

  Optional<User> findCommunityAdminById(String adminId);

  Optional<Community> getCommunityDetailsByIdWithAdmins(String communityId);

  Optional<Community> addAdminsToCommunity(String communityId, Set<String> admins);

  Set<String> addHousesToCommunity(String communityId, Set<CommunityHouse> houses);

```

---

</SwmSnippet>

<SwmSnippet path="/service/src/main/java/com/myhome/services/springdatajpa/CommunitySDJpaService.java" line="45">

---

# CommunityService Implementation

The CommunityService is implemented in the CommunitySDJpaService class. This class provides the actual business logic for the operations defined in the CommunityService interface.

```java
@Service
public class CommunitySDJpaService implements CommunityService {
  private final CommunityRepository communityRepository;
  private final UserRepository communityAdminRepository;
  private final CommunityMapper communityMapper;
  private final CommunityHouseRepository communityHouseRepository;
  private final HouseService houseService;

  @Override
  public Community createCommunity(CommunityDto communityDto) {
    communityDto.setCommunityId(generateUniqueId());
    String userId = (String) SecurityContextHolder.getContext().getAuthentication().getPrincipal();
    Community community = addAdminToCommunity(communityMapper.communityDtoToCommunity(communityDto),
        userId);
    Community savedCommunity = communityRepository.save(community);
    log.trace("saved community with id[{}] to repository", savedCommunity.getId());
    return savedCommunity;
  }

  private Community addAdminToCommunity(Community community, String userId) {
    communityAdminRepository.findByUserIdWithCommunities(userId).ifPresent(admin -> {
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBQ2l0aS1NeUhvbWUlM0ElM0FnaWxhZG5hdm90" repo-name="Citi-MyHome" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
