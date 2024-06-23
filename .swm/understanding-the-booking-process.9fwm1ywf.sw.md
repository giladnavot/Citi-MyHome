---
title: Understanding the Booking Process
---
Booking in Citi-MyHome refers to the process of reserving an amenity. The `BookingService` interface defines the contract for any service that wants to provide booking functionality, with the `deleteBooking` method allowing for the removal of a booking. The `BookingSDJpaService` class implements this interface, providing the logic to delete a booking from the repository. The `BookingController` class uses an instance of `BookingService` to handle HTTP requests related to bookings, such as deleting a booking.

<SwmSnippet path="/service/src/main/java/com/myhome/services/BookingService.java" line="3">

---

# BookingService Interface

The `BookingService` interface defines the contract for any class that wants to provide booking services. It declares a `deleteBooking` method, which takes an amenity ID and a booking ID as parameters.

```java
public interface BookingService {

  boolean deleteBooking(String amenityId, String bookingId);

}
```

---

</SwmSnippet>

<SwmSnippet path="/service/src/main/java/com/myhome/services/springdatajpa/BookingSDJpaService.java" line="11">

---

# BookingSDJpaService Class

The `BookingSDJpaService` class implements the `BookingService` interface. It provides the actual implementation of the `deleteBooking` method. This method deletes a booking from the `AmenityBookingItemRepository` if the amenity ID matches the one provided.

```java
@Service
@RequiredArgsConstructor
public class BookingSDJpaService implements BookingService {

  private final AmenityBookingItemRepository bookingRepository;

  @Transactional
  @Override
  public boolean deleteBooking(String amenityId, String bookingId) {
    Optional<AmenityBookingItem> booking =
        bookingRepository.findByAmenityBookingItemId(bookingId);
    return booking.map(bookingItem -> {
      boolean amenityFound =
          bookingItem.getAmenity().getAmenityId().equals(amenityId);
      if (amenityFound) {
        bookingRepository.delete(bookingItem);
        return true;
      } else {
        return false;
      }
    }).orElse(false);
```

---

</SwmSnippet>

<SwmSnippet path="/service/src/main/java/com/myhome/controllers/BookingController.java" line="12">

---

# BookingController Class

The `BookingController` class uses the `BookingService` to manage bookings. It has a `deleteBooking` method that is exposed as an API endpoint. This method uses the `deleteBooking` method of the `BookingService` to delete a booking and returns an appropriate HTTP response.

```java
@RestController
@Slf4j
@RequiredArgsConstructor
public class BookingController implements BookingsApi {

  private final BookingService bookingSDJpaService;

  @Override
  public ResponseEntity<Void> deleteBooking(@PathVariable String amenityId,
      @PathVariable String bookingId) {
    boolean isBookingDeleted = bookingSDJpaService.deleteBooking(amenityId, bookingId);
    if (isBookingDeleted) {
      return ResponseEntity.status(HttpStatus.NO_CONTENT).build();
    } else {
      return ResponseEntity.status(HttpStatus.NOT_FOUND).build();
    }
  }
}
```

---

</SwmSnippet>

# Booking Functionality

This section will focus on the `deleteBooking` function, which is a key part of the Booking functionality in the Citi-MyHome project.

<SwmSnippet path="/service/src/main/java/com/myhome/controllers/BookingController.java" line="19">

---

## deleteBooking

The `deleteBooking` function in the `BookingController` class is responsible for handling the deletion of a booking. It takes in an amenity ID and a booking ID, and uses the `deleteBooking` function from the `BookingService` to delete the booking.

```java
  @Override
  public ResponseEntity<Void> deleteBooking(@PathVariable String amenityId,
      @PathVariable String bookingId) {
    boolean isBookingDeleted = bookingSDJpaService.deleteBooking(amenityId, bookingId);
    if (isBookingDeleted) {
      return ResponseEntity.status(HttpStatus.NO_CONTENT).build();
    } else {
      return ResponseEntity.status(HttpStatus.NOT_FOUND).build();
    }
  }
```

---

</SwmSnippet>

<SwmSnippet path="/service/src/main/java/com/myhome/services/springdatajpa/BookingSDJpaService.java" line="19">

---

The `deleteBooking` function in the `BookingSDJpaService` class is where the actual deletion of the booking takes place. It first finds the booking by its ID, checks if the amenity ID matches, and then deletes the booking if it does.

```java
  public boolean deleteBooking(String amenityId, String bookingId) {
    Optional<AmenityBookingItem> booking =
        bookingRepository.findByAmenityBookingItemId(bookingId);
    return booking.map(bookingItem -> {
      boolean amenityFound =
          bookingItem.getAmenity().getAmenityId().equals(amenityId);
      if (amenityFound) {
        bookingRepository.delete(bookingItem);
        return true;
      } else {
        return false;
      }
    }).orElse(false);
  }
```

---

</SwmSnippet>

<SwmSnippet path="/service/src/main/java/com/myhome/services/BookingService.java" line="5">

---

The `deleteBooking` function is declared in the `BookingService` interface, which is implemented by the `BookingSDJpaService` class.

```java
  boolean deleteBooking(String amenityId, String bookingId);
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBQ2l0aS1NeUhvbWUlM0ElM0FnaWxhZG5hdm90" repo-name="Citi-MyHome" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
