---
title: Basic Concepts of House in Citi-MyHome
---
In Citi-MyHome, a 'House' refers to an entity that represents a physical house in a community. It is managed through the `HouseService` interface, which provides methods for listing all houses, adding members to a house, deleting members from a house, and retrieving house details by ID. The `HouseController` class uses the `HouseService` to handle HTTP requests related to houses. The `CommunityHouse` class in the `com.myhome.domain` package represents the data model for a house.

<SwmSnippet path="/service/src/main/java/com/myhome/controllers/HouseController.java" line="43">

---

# HouseController

The HouseController class provides the API endpoints for the house-related operations. It uses the HouseService to perform the operations.

```java
@RestController
@RequiredArgsConstructor
@Slf4j
public class HouseController implements HousesApi {
  private final HouseMemberMapper houseMemberMapper;
  private final HouseService houseService;
  private final HouseApiMapper houseApiMapper;

  @Override
  public ResponseEntity<GetHouseDetailsResponse> listAllHouses(
      @PageableDefault(size = 200) Pageable pageable) {
    log.trace("Received request to list all houses");

    Set<CommunityHouse> houseDetails =
        houseService.listAllHouses(pageable);
    Set<GetHouseDetailsResponseCommunityHouse> getHouseDetailsResponseSet =
        houseApiMapper.communityHouseSetToRestApiResponseCommunityHouseSet(houseDetails);

    GetHouseDetailsResponse response = new GetHouseDetailsResponse();

    response.setHouses(getHouseDetailsResponseSet);
```

---

</SwmSnippet>

<SwmSnippet path="/service/src/main/java/com/myhome/services/HouseService.java" line="26">

---

# HouseService

The HouseService interface provides the necessary methods for interacting with house data. These methods are implemented by the HouseSDJpaService class.

```java
public interface HouseService {
  Set<CommunityHouse> listAllHouses();

  Set<CommunityHouse> listAllHouses(Pageable pageable);

  Set<HouseMember> addHouseMembers(String houseId, Set<HouseMember> houseMembers);

  boolean deleteMemberFromHouse(String houseId, String memberId);

  Optional<CommunityHouse> getHouseDetailsById(String houseId);

  Optional<List<HouseMember>> getHouseMembersById(String houseId, Pageable pageable);

  Optional<List<HouseMember>> listHouseMembersForHousesOfUserId(String userId, Pageable pageable);
}
```

---

</SwmSnippet>

<SwmSnippet path="/service/src/main/java/com/myhome/services/springdatajpa/HouseSDJpaService.java" line="36">

---

# HouseSDJpaService

The HouseSDJpaService class implements the HouseService interface, providing the actual implementation of the house-related operations.

```java
@Service
public class HouseSDJpaService implements HouseService {
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBQ2l0aS1NeUhvbWUlM0ElM0FnaWxhZG5hdm90" repo-name="Citi-MyHome" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
