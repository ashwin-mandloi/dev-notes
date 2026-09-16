---
title: SOLID Principles
sidebar_position: 3
---

# SOLID Principles in Java

SOLID is a set of five object-oriented design principles that help make software easier to understand, maintain, test, and extend.

The word **SOLID** is formed from the first letter of each principle:

1. Single Responsibility Principle  
2. Open/Closed Principle  
3. Liskov Substitution Principle  
4. Interface Segregation Principle  
5. Dependency Inversion Principle  

## 1. Single Responsibility Principle (SRP)

The Single Responsibility Principle states that a class should have only one responsibility or one reason to change.

A class should not handle multiple unrelated tasks, such as storing user data, validating data, sending emails, and generating reports. Each responsibility should be placed in a separate class.

**Real-world Example:** In a school, a teacher teaches students, an accountant manages fees, and a librarian manages books. One person should not be responsible for every task.

**How it is implemented in Java:** Separate different responsibilities into different classes.

```java
class User {
    private String name;
    private String email;

    User(String name, String email) {
        this.name = name;
        this.email = email;
    }

    public String getName() {
        return name;
    }

    public String getEmail() {
        return email;
    }
}
```

```java
class UserRepository {
    void save(User user) {
        System.out.println("Saving user: " + user.getName());
    }
}
```

```java
class EmailService {
    void sendWelcomeEmail(User user) {
        System.out.println("Sending welcome email to: " + user.getEmail());
    }
}
```

In this example:

- `User` stores user information.
- `UserRepository` is responsible for saving users.
- `EmailService` is responsible for sending emails.

Each class has one clear responsibility.

## 2. Open/Closed Principle (OCP)

The Open/Closed Principle states that a class should be open for extension but closed for modification.

This means existing code should not need to be changed every time a new feature is added. Instead, new behavior should be added by creating new classes that extend or implement existing abstractions.

**Real-world Example:** A payment system can support new payment methods without changing its existing payment-processing code.

**How it is implemented in Java:** Use interfaces or abstract classes with polymorphism.

```java
interface PaymentMethod {
    void pay(double amount);
}
```

```java
class CardPayment implements PaymentMethod {
    @Override
    public void pay(double amount) {
        System.out.println("Paid " + amount + " using a card.");
    }
}
```

```java
class UpiPayment implements PaymentMethod {
    @Override
    public void pay(double amount) {
        System.out.println("Paid " + amount + " using UPI.");
    }
}
```

```java
class PaymentProcessor {
    void processPayment(PaymentMethod paymentMethod, double amount) {
        paymentMethod.pay(amount);
    }
}
```

```java
PaymentProcessor processor = new PaymentProcessor();

processor.processPayment(new CardPayment(), 500);
processor.processPayment(new UpiPayment(), 750);
```

A new payment method can be added without modifying `PaymentProcessor`.

```java
class CashPayment implements PaymentMethod {
    @Override
    public void pay(double amount) {
        System.out.println("Paid " + amount + " using cash.");
    }
}
```

This follows the Open/Closed Principle because the system is extended by adding a new class instead of changing existing working code.

## 3. Liskov Substitution Principle (LSP)

The Liskov Substitution Principle states that a child class should be able to replace its parent class without causing incorrect behavior.

If a program works with a parent class, it should also work correctly when given any valid child class.

**Real-world Example:** If a system expects a vehicle, it should work correctly with a car, bike, or bus, as long as each one behaves like a vehicle.

**How it is implemented in Java:** Child classes should honor the expected behavior of their parent class or interface.

```java
class Vehicle {
    void start() {
        System.out.println("Vehicle started.");
    }
}
```

```java
class Car extends Vehicle {
    @Override
    void start() {
        System.out.println("Car started.");
    }
}
```

```java
class Motorcycle extends Vehicle {
    @Override
    void start() {
        System.out.println("Motorcycle started.");
    }
}
```

```java
class VehicleService {
    void startVehicle(Vehicle vehicle) {
        vehicle.start();
    }
}
```

```java
VehicleService service = new VehicleService();

service.startVehicle(new Car());
service.startVehicle(new Motorcycle());
```

Both `Car` and `Motorcycle` can replace `Vehicle` because they provide valid behavior for `start()`.

A child class should not change the meaning of an inherited method in an unexpected way. For example, if a parent class promises that a method can perform an action, a child class should not override that method only to throw an unsupported-operation error.

## 4. Interface Segregation Principle (ISP)

The Interface Segregation Principle states that a class should not be forced to implement methods that it does not need.

Large interfaces with many unrelated methods should be divided into smaller, focused interfaces.

**Real-world Example:** A basic printer can print documents, but it may not scan or fax. It should not be forced to implement scanning and faxing features.

**How it is implemented in Java:** Create separate interfaces for separate capabilities.

```java
interface Printable {
    void print();
}
```

```java
interface Scannable {
    void scan();
}
```

```java
interface Faxable {
    void fax();
}
```

```java
class BasicPrinter implements Printable {
    @Override
    public void print() {
        System.out.println("Printing document.");
    }
}
```

```java
class MultiFunctionPrinter implements Printable, Scannable, Faxable {
    @Override
    public void print() {
        System.out.println("Printing document.");
    }

    @Override
    public void scan() {
        System.out.println("Scanning document.");
    }

    @Override
    public void fax() {
        System.out.println("Sending fax.");
    }
}
```

In this example:

- `BasicPrinter` implements only `Printable`.
- `MultiFunctionPrinter` implements all the features it supports.
- No class is forced to implement unnecessary methods.

## 5. Dependency Inversion Principle (DIP)

The Dependency Inversion Principle states that high-level classes should not depend directly on low-level classes. Both should depend on abstractions.

In simple terms, a class should depend on an interface or abstract class instead of directly creating and using a specific concrete class.

**Real-world Example:** A switch should work with any compatible device, such as a bulb or fan. The switch should not be designed only for one specific bulb.

**How it is implemented in Java:** Use interfaces and pass dependencies through constructors or methods.

```java
interface MessageService {
    void sendMessage(String message);
}
```

```java
class EmailMessageService implements MessageService {
    @Override
    public void sendMessage(String message) {
        System.out.println("Sending email: " + message);
    }
}
```

```java
class SmsMessageService implements MessageService {
    @Override
    public void sendMessage(String message) {
        System.out.println("Sending SMS: " + message);
    }
}
```

```java
class NotificationService {
    private final MessageService messageService;

    NotificationService(MessageService messageService) {
        this.messageService = messageService;
    }

    void notifyUser(String message) {
        messageService.sendMessage(message);
    }
}
```

```java
MessageService emailService = new EmailMessageService();
NotificationService notificationService = new NotificationService(emailService);

notificationService.notifyUser("Your order has been placed.");
```

The `NotificationService` depends on the `MessageService` interface, not directly on `EmailMessageService`.

The message service can be changed without modifying `NotificationService`.

```java
MessageService smsService = new SmsMessageService();
NotificationService notificationService = new NotificationService(smsService);

notificationService.notifyUser("Your order has been placed.");
```

## How the SOLID Principles Work Together

The following example combines all five SOLID principles:

```java
interface PaymentMethod {
    void pay(double amount);
}
```

```java
class CardPayment implements PaymentMethod {
    @Override
    public void pay(double amount) {
        System.out.println("Paid " + amount + " using a card.");
    }
}
```

```java
class UpiPayment implements PaymentMethod {
    @Override
    public void pay(double amount) {
        System.out.println("Paid " + amount + " using UPI.");
    }
}
```

```java
class Order {
    private final double amount;

    Order(double amount) {
        this.amount = amount;
    }

    public double getAmount() {
        return amount;
    }
}
```

```java
class OrderPaymentService {
    private final PaymentMethod paymentMethod;

    OrderPaymentService(PaymentMethod paymentMethod) {
        this.paymentMethod = paymentMethod;
    }

    void payForOrder(Order order) {
        paymentMethod.pay(order.getAmount());
    }
}
```

```java
Order order = new Order(1200);

PaymentMethod paymentMethod = new UpiPayment();
OrderPaymentService paymentService = new OrderPaymentService(paymentMethod);

paymentService.payForOrder(order);
```

In this example:

- **Single Responsibility Principle:** `Order` stores order data, while `OrderPaymentService` processes payments.
- **Open/Closed Principle:** New payment methods can be added by implementing `PaymentMethod`.
- **Liskov Substitution Principle:** Any valid `PaymentMethod` implementation can replace another.
- **Interface Segregation Principle:** `PaymentMethod` contains only one focused responsibility.
- **Dependency Inversion Principle:** `OrderPaymentService` depends on the `PaymentMethod` interface instead of a specific payment class.

## Benefits of Using SOLID Principles

- Makes code easier to read and maintain.
- Reduces the effect of changes in one part of the program.
- Makes classes easier to test.
- Improves code reuse.
- Supports flexible and scalable application design.
- Reduces tightly coupled code.