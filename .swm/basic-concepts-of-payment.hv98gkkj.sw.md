---
title: Basic Concepts of Payment
---
Payment in Citi-MyHome refers to the functionality that allows users to schedule, retrieve, and manage payments. It is primarily handled by the `PaymentService` interface, which defines methods for scheduling a payment, retrieving payment details, and getting payments by a member or admin. The `PaymentController` class implements these functionalities and provides endpoints for the client to interact with. The `PaymentRepository` interface is used to interact with the database, providing methods to find and delete payments by their ID.

<SwmSnippet path="/service/src/main/java/com/myhome/services/PaymentService.java" line="30">

---

# PaymentService Interface

The PaymentService interface defines the methods that are used to handle payment-related operations. These include `schedulePayment` for scheduling a payment, `getPaymentDetails` for retrieving payment details, `getPaymentsByMember` for listing all payments made by a specific member, and `getPaymentsByAdmin` for listing all payments made by a specific admin.

```java
public interface PaymentService {
  PaymentDto schedulePayment(PaymentDto request);

  Optional<PaymentDto> getPaymentDetails(String paymentId);

  Set<Payment> getPaymentsByMember(String memberId);

  Page<Payment> getPaymentsByAdmin(String adminId, Pageable pageable);

  Optional<HouseMember> getHouseMember(String memberId);
}
```

---

</SwmSnippet>

<SwmSnippet path="/service/src/main/java/com/myhome/controllers/PaymentController.java" line="54">

---

# PaymentController Class

The PaymentController class implements the PaymentsApi and uses the PaymentService to handle HTTP requests related to payments. It includes methods for scheduling a payment, listing payment details, listing all payments made by a specific member, and listing all payments made by a specific admin.

```java
public class PaymentController implements PaymentsApi {
  private final PaymentService paymentService;
  private final CommunityService communityService;
  private final SchedulePaymentApiMapper schedulePaymentApiMapper;

  @Override
  public ResponseEntity<SchedulePaymentResponse> schedulePayment(@Valid
      SchedulePaymentRequest request) {
    log.trace("Received schedule payment request");

    HouseMember houseMember = paymentService.getHouseMember(request.getMemberId())
        .orElseThrow(() -> new RuntimeException(
            "House member with given id not exists: " + request.getMemberId()));
    User admin = communityService.findCommunityAdminById(request.getAdminId())
        .orElseThrow(
            () -> new RuntimeException("Admin with given id not exists: " + request.getAdminId()));

    if (isUserAdminOfCommunityHouse(houseMember.getCommunityHouse(), admin)) {
      final EnrichedSchedulePaymentRequest paymentRequest =
          schedulePaymentApiMapper.enrichSchedulePaymentRequest(request, admin, houseMember);
      final PaymentDto paymentDto =
```

---

</SwmSnippet>

<SwmSnippet path="/service/src/main/java/com/myhome/services/springdatajpa/PaymentSDJpaService.java" line="47">

---

# PaymentSDJpaService Class

The PaymentSDJpaService class implements the PaymentService interface and uses the PaymentRepository to interact with the database. It includes implementations for all the methods defined in the PaymentService interface.

```java
public class PaymentSDJpaService implements PaymentService {
  private final PaymentRepository paymentRepository;
  private final UserRepository adminRepository;
  private final PaymentMapper paymentMapper;
  private final HouseMemberRepository houseMemberRepository;

  @Override
  public PaymentDto schedulePayment(PaymentDto request) {
    generatePaymentId(request);
    return createPaymentInRepository(request);
  }

  @Override
  public Optional<PaymentDto> getPaymentDetails(String paymentId) {
    return paymentRepository.findByPaymentId(paymentId)
        .map(paymentMapper::paymentToPaymentDto);
  }

  @Override
  public Optional<HouseMember> getHouseMember(String memberId) {
    return houseMemberRepository.findByMemberId(memberId);
```

---

</SwmSnippet>

# Payment Functions

This section will cover the main functions related to payment processing in the Citi-MyHome project.

<SwmSnippet path="/service/src/main/java/com/myhome/services/PaymentService.java" line="31">

---

## schedulePayment

The `schedulePayment` function is used to schedule a new payment. It takes a PaymentDto object as input, which contains the details of the payment to be scheduled.

```java
  PaymentDto schedulePayment(PaymentDto request);
```

---

</SwmSnippet>

<SwmSnippet path="/service/src/main/java/com/myhome/services/PaymentService.java" line="33">

---

## getPaymentDetails

The `getPaymentDetails` function is used to retrieve the details of a specific payment. It takes a payment ID as input and returns an Optional<PaymentDto> object.

```java
  Optional<PaymentDto> getPaymentDetails(String paymentId);
```

---

</SwmSnippet>

<SwmSnippet path="/service/src/main/java/com/myhome/services/PaymentService.java" line="35">

---

## getPaymentsByMember

The `getPaymentsByMember` function is used to retrieve all payments made by a specific member. It takes a member ID as input and returns a Set of Payment objects.

```java
  Set<Payment> getPaymentsByMember(String memberId);
```

---

</SwmSnippet>

<SwmSnippet path="/service/src/main/java/com/myhome/services/PaymentService.java" line="37">

---

## getPaymentsByAdmin

The `getPaymentsByAdmin` function is used to retrieve all payments overseen by a specific admin. It takes an admin ID and a Pageable object as input, and returns a Page of Payment objects.

```java
  Page<Payment> getPaymentsByAdmin(String adminId, Pageable pageable);
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBQ2l0aS1NeUhvbWUlM0ElM0FnaWxhZG5hdm90" repo-name="Citi-MyHome" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
