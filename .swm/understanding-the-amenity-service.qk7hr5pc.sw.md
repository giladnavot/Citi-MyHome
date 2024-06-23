---
title: Understanding the Amenity Service
---
Amenity in Citi-MyHome refers to the various facilities or services provided within a community. It is represented by the `AmenityService` interface which defines methods for creating, retrieving, deleting, and updating amenities. The `AmenityController` class uses this service to handle HTTP requests related to amenities. The actual implementation of the `AmenityService` interface is provided by the `AmenitySDJpaService` class. The `AmenityRepository` is used to interact with the database for operations related to amenities.

<SwmSnippet path="/service/src/main/java/com/myhome/services/AmenityService.java" line="25">

---

# AmenityService Interface

The 'AmenityService' interface defines the methods for managing amenities. These include 'createAmenities', 'getAmenityDetails', 'deleteAmenity', 'listAllAmenities', and 'updateAmenity'.

```java
public interface AmenityService {

  Optional<List<AmenityDto>> createAmenities(Set<AmenityDto> amenities, String communityId);

  Optional<Amenity> getAmenityDetails(String amenityId);

  boolean deleteAmenity(String amenityId);

  Set<Amenity> listAllAmenities(String communityId);

  boolean updateAmenity(AmenityDto updatedAmenityDto);
}
```

---

</SwmSnippet>

<SwmSnippet path="/service/src/main/java/com/myhome/services/springdatajpa/AmenitySDJpaService.java" line="37">

---

# AmenitySDJpaService Class

The 'AmenitySDJpaService' class implements the 'AmenityService' interface, providing the actual functionality for the methods defined in the interface.

```java
public class AmenitySDJpaService implements AmenityService {
```

---

</SwmSnippet>

<SwmSnippet path="/service/src/main/java/com/myhome/controllers/AmenityController.java" line="38">

---

# AmenityController Class

The 'AmenityController' class uses the 'AmenityService' to provide endpoints for the API. It includes methods for getting amenity details, listing all amenities, adding an amenity to a community, deleting an amenity, and updating an amenity.

```java
@RestController
@Slf4j
@RequiredArgsConstructor
public class AmenityController implements AmenitiesApi {

  private final AmenityService amenitySDJpaService;
  private final AmenityApiMapper amenityApiMapper;

  @Override
  public ResponseEntity<GetAmenityDetailsResponse> getAmenityDetails(
      @PathVariable String amenityId) {
    return amenitySDJpaService.getAmenityDetails(amenityId)
        .map(amenityApiMapper::amenityToAmenityDetailsResponse)
        .map(ResponseEntity::ok)
        .orElse(ResponseEntity.status(HttpStatus.NOT_FOUND).build());
  }

  @Override
  public ResponseEntity<Set<GetAmenityDetailsResponse>> listAllAmenities(
      @PathVariable String communityId) {
    Set<Amenity> amenities = amenitySDJpaService.listAllAmenities(communityId);
```

---

</SwmSnippet>

# Amenity Functions

This section will cover the main functions related to the Amenity feature in the Citi-MyHome project.

<SwmSnippet path="/service/src/main/java/com/myhome/services/springdatajpa/AmenitySDJpaService.java" line="44">

---

## createAmenities

The `createAmenities` function is responsible for creating new amenities. It takes a set of AmenityDto objects and a communityId as parameters. The function first retrieves the community using the communityId. If the community is not found, it returns an empty Optional. If the community is found, it maps each AmenityDto to an Amenity, sets the community for each amenity, and saves them in the repository.

```java
  @Override
  public Optional<List<AmenityDto>> createAmenities(Set<AmenityDto> amenities, String communityId) {
    final Optional<Community> community = communityService.getCommunityDetailsById(communityId);
    if (!community.isPresent()) {
      return Optional.empty();
    }
    final List<Amenity> amenitiesWithCommunity = amenities.stream()
        .map(amenityApiMapper::amenityDtoToAmenity)
        .map(amenity -> {
          amenity.setCommunity(community.get());
          return amenity;
        })
        .collect(Collectors.toList());
    final List<AmenityDto> createdAmenities =
        amenityRepository.saveAll(amenitiesWithCommunity).stream()
            .map(amenityApiMapper::amenityToAmenityDto)
            .collect(Collectors.toList());
    return Optional.of(createdAmenities);
  }
```

---

</SwmSnippet>

<SwmSnippet path="/service/src/main/java/com/myhome/services/springdatajpa/AmenitySDJpaService.java" line="64">

---

## getAmenityDetails

The `getAmenityDetails` function retrieves the details of a specific amenity using its ID. It calls the `findByAmenityId` method from the AmenityRepository to retrieve the amenity.

```java
  @Override
  public Optional<Amenity> getAmenityDetails(String amenityId) {
    return amenityRepository.findByAmenityId(amenityId);
  }
```

---

</SwmSnippet>

<SwmSnippet path="/service/src/main/java/com/myhome/services/springdatajpa/AmenitySDJpaService.java" line="69">

---

## deleteAmenity

The `deleteAmenity` function deletes a specific amenity using its ID. It first retrieves the amenity along with its associated community. If the amenity is found, it is removed from the community's amenities and then deleted from the repository.

```java
  @Override
  public boolean deleteAmenity(String amenityId) {
    return amenityRepository.findByAmenityIdWithCommunity(amenityId)
        .map(amenity -> {
          Community community = amenity.getCommunity();
          community.getAmenities().remove(amenity);
          amenityRepository.delete(amenity);
          return true;
        })
        .orElse(false);
  }
```

---

</SwmSnippet>

<SwmSnippet path="/service/src/main/java/com/myhome/services/springdatajpa/AmenitySDJpaService.java" line="81">

---

## listAllAmenities

The `listAllAmenities` function retrieves all amenities for a specific community using the communityId. It calls the `findByCommunityIdWithAmenities` method from the CommunityRepository to retrieve the community and its amenities.

```java
  @Override
  public Set<Amenity> listAllAmenities(String communityId) {
    return communityRepository.findByCommunityIdWithAmenities(communityId)
        .map(Community::getAmenities)
        .orElse(new HashSet<>());
  }
```

---

</SwmSnippet>

<SwmSnippet path="/service/src/main/java/com/myhome/services/springdatajpa/AmenitySDJpaService.java" line="88">

---

## updateAmenity

The `updateAmenity` function updates a specific amenity using an AmenityDto object. It first retrieves the amenity using its ID. If the amenity is found, it updates the amenity's details and saves it in the repository.

```java
  @Override
  public boolean updateAmenity(AmenityDto updatedAmenity) {
    String amenityId = updatedAmenity.getAmenityId();
    return amenityRepository.findByAmenityId(amenityId)
        .map(amenity -> communityRepository.findByCommunityId(updatedAmenity.getCommunityId())
            .map(community -> {
              Amenity updated = new Amenity();
              updated.setName(updatedAmenity.getName());
              updated.setPrice(updatedAmenity.getPrice());
              updated.setId(amenity.getId());
              updated.setAmenityId(amenityId);
              updated.setDescription(updatedAmenity.getDescription());
              return updated;
            })
            .orElse(null))
        .map(amenityRepository::save).isPresent();
  }
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBQ2l0aS1NeUhvbWUlM0ElM0FnaWxhZG5hdm90" repo-name="Citi-MyHome" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
