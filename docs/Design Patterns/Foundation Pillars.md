---
title: OOP - Foundation Pillars
---

Object-Oriented Programming is a programming approach that organizes code around objects. An object contains data, called **fields**, and behavior, called **methods**.

The four foundation pillars of OOP are:

1. Encapsulation  
2. Abstraction  
3. Inheritance  
4. Polymorphism  

## 1. Encapsulation

Encapsulation means combining data and the methods that work with that data inside one class. It also means restricting direct access to important data.

In Java, fields are commonly marked as `private`. Other classes cannot directly change them. Instead, controlled methods such as getters and setters are used to read or update the data.

**Real-world Example:** A bank account should not allow anyone to directly change its balance. Deposits and withdrawals should happen through controlled methods.

**How it is implemented in Java:** Use `private` fields and public methods to control access.

```java
class BankAccount {
    private String accountHolder;
    private double balance;

    BankAccount(String accountHolder, double openingBalance) {
        this.accountHolder = accountHolder;
        this.balance = openingBalance;
    }

    public double getBalance() {
        return balance;
    }

    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }

    public void withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
        }
    }
}
```

In this example:

- `balance` is private, so outside classes cannot directly set it.
- `deposit()` ensures that only positive amounts are added.
- `withdraw()` prevents withdrawing more money than is available.
- `getBalance()` provides read-only access to the current balance.

Encapsulation improves security, reduces accidental changes, and makes code easier to maintain.

## 2. Abstraction

Abstraction means showing only the essential features of an object while hiding unnecessary internal details. It allows a user of a class to know **what** an object does without needing to know **how** it does it.

In Java, abstraction is commonly implemented using abstract classes and interfaces.

**Real-world Example:** A person can drive a car by using the steering wheel, pedals, and controls without knowing how the engine or transmission works internally.

**How it is implemented in Java:** Use an abstract class when related classes share common behavior but must provide their own implementation of certain methods.

```java
abstract class Vehicle {
    abstract void start();

    void stop() {
        System.out.println("Vehicle stopped.");
    }
}

class Car extends Vehicle {
    @Override
    void start() {
        System.out.println("Car starts with a key or button.");
    }
}

class Motorcycle extends Vehicle {
    @Override
    void start() {
        System.out.println("Motorcycle starts with an ignition switch.");
    }
}
```

In this example:

- `Vehicle` is an abstract class, so objects cannot be created directly from it.
- `start()` is abstract because each vehicle starts differently.
- `stop()` is a shared concrete method available to every vehicle.
- `Car` and `Motorcycle` provide their own implementation of `start()`.

An interface is another way to achieve abstraction. It defines a contract that classes agree to follow.

```java
interface Payment {
    void pay(double amount);
}

class CardPayment implements Payment {
    @Override
    public void pay(double amount) {
        System.out.println("Paid " + amount + " using a card.");
    }
}
```

Abstraction makes large programs easier to understand because it separates essential behavior from implementation details.

## 3. Inheritance

Inheritance allows one class to inherit fields and methods from another class. The existing class is called the **parent class**, **superclass**, or **base class**. The new class is called the **child class**, **subclass**, or **derived class**.

Inheritance represents an **is-a** relationship.

**Real-world Example:** A dog is an animal. A dog can use general animal behavior, such as eating and sleeping, while also having its own behavior, such as barking.

**How it is implemented in Java:** Use the `extends` keyword.

```java
class Animal {
    String name;

    Animal(String name) {
        this.name = name;
    }

    void eat() {
        System.out.println(name + " is eating.");
    }

    void sleep() {
        System.out.println(name + " is sleeping.");
    }
}

class Dog extends Animal {
    Dog(String name) {
        super(name);
    }

    void bark() {
        System.out.println(name + " says Woof!");
    }
}
```

In this example:

- `Dog` inherits the `name`, `eat()`, and `sleep()` members from `Animal`.
- `Dog` adds its own `bark()` method.
- `super(name)` calls the constructor of the parent class.

```java
Dog dog = new Dog("Buddy");

dog.eat();    // Inherited from Animal
dog.sleep();  // Inherited from Animal
dog.bark();   // Defined in Dog
```

Inheritance supports code reuse and helps create clear class hierarchies. Java supports single inheritance for classes, meaning a class can extend only one class directly. However, a class can implement multiple interfaces.

## 4. Polymorphism

Polymorphism means “many forms.” It allows the same method name, parent reference, or interface reference to behave differently depending on the actual object involved.

Polymorphism in Java is commonly divided into two types:

1. Compile-time polymorphism, achieved through method overloading.
2. Runtime polymorphism, achieved through method overriding.

### Method Overloading

Method overloading happens when a class has multiple methods with the same name but different parameter lists. The compiler chooses the correct method based on the arguments provided.

```java
class Calculator {
    int add(int firstNumber, int secondNumber) {
        return firstNumber + secondNumber;
    }

    double add(double firstNumber, double secondNumber) {
        return firstNumber + secondNumber;
    }

    int add(int firstNumber, int secondNumber, int thirdNumber) {
        return firstNumber + secondNumber + thirdNumber;
    }
}
```

In this example, the `add()` method has different forms:

```java
Calculator calculator = new Calculator();

System.out.println(calculator.add(5, 10));
System.out.println(calculator.add(2.5, 3.5));
System.out.println(calculator.add(1, 2, 3));
```

### Method Overriding

Method overriding happens when a child class provides its own version of a method inherited from a parent class. The method that runs is decided at runtime based on the actual object.

```java
class Animal {
    void makeSound() {
        System.out.println("Animal makes a sound.");
    }
}

class Dog extends Animal {
    @Override
    void makeSound() {
        System.out.println("Dog barks.");
    }
}

class Cat extends Animal {
    @Override
    void makeSound() {
        System.out.println("Cat meows.");
    }
}
```

```java
Animal firstAnimal = new Dog();
Animal secondAnimal = new Cat();

firstAnimal.makeSound();
secondAnimal.makeSound();
```

Output:

```text
Dog barks.
Cat meows.
```

In this example:

- Both variables use the parent type, `Animal`.
- Each variable refers to a different child object.
- Java chooses the correct overridden `makeSound()` method at runtime.

Polymorphism makes programs flexible because code can work with general parent or interface types instead of being written separately for every child class.

## How the Four Pillars Work Together

The following example uses all four OOP pillars together:

```java
abstract class Employee {
    private String name; // Encapsulation

    Employee(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    abstract double calculateSalary(); // Abstraction
}

class FullTimeEmployee extends Employee { // Inheritance
    private double monthlySalary;

    FullTimeEmployee(String name, double monthlySalary) {
        super(name);
        this.monthlySalary = monthlySalary;
    }

    @Override
    double calculateSalary() { // Polymorphism
        return monthlySalary;
    }
}

class PartTimeEmployee extends Employee { // Inheritance
    private int hoursWorked;
    private double hourlyRate;

    PartTimeEmployee(String name, int hoursWorked, double hourlyRate) {
        super(name);
        this.hoursWorked = hoursWorked;
        this.hourlyRate = hourlyRate;
    }

    @Override
    double calculateSalary() { // Polymorphism
        return hoursWorked * hourlyRate;
    }
}
```

```java
Employee firstEmployee = new FullTimeEmployee("Asha", 50000);
Employee secondEmployee = new PartTimeEmployee("Rohan", 80, 300);

System.out.println(firstEmployee.getName() + ": " + firstEmployee.calculateSalary());
System.out.println(secondEmployee.getName() + ": " + secondEmployee.calculateSalary());
```

In this example:

- **Encapsulation:** The `name` field is private and accessed through `getName()`.
- **Abstraction:** `Employee` defines the abstract `calculateSalary()` method.
- **Inheritance:** `FullTimeEmployee` and `PartTimeEmployee` extend `Employee`.
- **Polymorphism:** Both employee objects use the `Employee` type, but calculate salary differently.