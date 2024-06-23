---
title: Overview of Mail
---
Mail in Citi-MyHome refers to the functionality provided by the MailService interface and its implementations. It is responsible for sending different types of emails to users, such as password recovery, account creation, password change, and account confirmation. The emails are sent using templates, which are filled with data such as the user's name, recovery code, or account confirmation link. The actual sending of the emails is handled by the JavaMailSender, which is part of the Spring Framework.

<SwmSnippet path="/service/src/main/java/com/myhome/services/MailService.java" line="6">

---

# MailService Interface

The MailService interface defines the methods for sending different types of emails. Each method takes a User object and additional parameters depending on the type of email to be sent.

```java
public interface MailService {

  boolean sendPasswordRecoverCode(User user, String randomCode);

  boolean sendAccountCreated(User user, SecurityToken emailConfirmToken);

  boolean sendPasswordSuccessfullyChanged(User user);

  boolean sendAccountConfirmed(User user);
}
```

---

</SwmSnippet>

<SwmSnippet path="/service/src/main/java/com/myhome/services/springdatajpa/MailSDJpaService.java" line="30">

---

# MailSDJpaService Class

The MailSDJpaService class implements the MailService interface. It uses the JavaMailSender for sending emails and Thymeleaf for processing email templates. Each method defined in the MailService interface is implemented here. The methods create a template model, get the localized subject line, and call the send method with the appropriate parameters.

```java
public class MailSDJpaService implements MailService {

  private final ITemplateEngine emailTemplateEngine;
  private final JavaMailSender mailSender;
  private final ResourceBundleMessageSource messageSource;
  private final MailProperties mailProperties;

  @Override
  public boolean sendPasswordRecoverCode(User user, String randomCode) {
    Map<String, Object> templateModel = new HashMap<>();
    templateModel.put("username", user.getName());
    templateModel.put("recoverCode", randomCode);
    String passwordRecoverSubject = getLocalizedMessage("locale.EmailSubject.passwordRecover");
    boolean mailSent = send(user.getEmail(), passwordRecoverSubject,
        MailTemplatesNames.PASSWORD_RESET.filename, templateModel);
    return mailSent;
  }

  @Override
  public boolean sendPasswordSuccessfullyChanged(User user) {
    Map<String, Object> templateModel = new HashMap<>();
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBQ2l0aS1NeUhvbWUlM0ElM0FnaWxhZG5hdm90" repo-name="Citi-MyHome" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
