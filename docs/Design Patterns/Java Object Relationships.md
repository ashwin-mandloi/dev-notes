---
title: OOP - Relationships in Java
sidebar_position: 2
---

## 1. Dependency (The "Knows About" Relationship)

Dependency is a temporary connection. It means one class uses another class inside a single method, often just to read data or trigger a quick action. The connection only lasts while that specific method is running.

**Real-world Example:** A Driver uses a Map to check directions. Once they know where to go, they put the map away.

**How it looks in Java:** You instantiate or use an object locally inside a method.

```java
class Map {
    void showRoute() {
        System.out.println("Path found.");
    }
}

class Driver {
    void navigate() {
        // Temporary connection: Map is created and used only inside this method
        Map map = new Map();
        map.showRoute();
    }
}
```

## 2. Association (The "Using" Relationship)

Association means two separate classes interact to do a job, but they remain entirely independent. There is no instance field holding the object inside either class. They simply communicate by passing messages.

**Real-world Example:** A Doctor and a Patient. A doctor treats a patient, and a patient visits a doctor. They interact during an appointment, but the doctor does not permanently own or store the patient.

**How it looks in Java:** Objects interact purely by being passed in as method parameters.

```java
class Patient {
    void receiveTreatment() {
        System.out.println("Patient is treated.");
    }
}

class Doctor {
    // Pure Association: No 'Patient' field at the top of the class.
    // The connection happens dynamically through the parameter.
    void treat(Patient patient) {
        patient.receiveTreatment();
    }
}
```

## 3. Aggregation (The "Has-A" Relationship)

Aggregation is where one large object has smaller objects as permanent fields, but those smaller objects can easily survive on their own. It is a loose form of ownership.

**Real-world Example:** A Department and a Teacher. The department has teachers. If the department closes down, the teachers do not vanish; they can go join another department.

**How it looks in Java:** The separate object is created outside the main class and is passed in through a constructor or a setter to be stored in an instance field. In spring boot autowired fields are usually the example of aggregation.

```java
class Teacher {
    String name;

    Teacher(String name) {
        this.name = name;
    }
}

class Department {
    private Teacher teacher; // Instance field establishes ownership

    // Teacher is created somewhere else and passed in
    Department(Teacher teacher) {
        this.teacher = teacher;
    }
}
```

## 4. Composition (The "Part-Of" Relationship)

Composition is a very strict and strong form of aggregation. The inner object is a fundamental part of the outer object. They share the same lifespan; if the main object is destroyed, the inner objects are instantly destroyed too.

**Real-world Example:** A Car and an Engine. The engine is built directly into the car. If the car is crushed and scrapped, the engine goes away with it.

**How it looks in Java:** The inner object is created inside the main class's constructor. It cannot exist without the outer class.

```java
class Engine {
    void start() {
        System.out.println("Vroom!");
    }
}

class Car {
    private final Engine engine; // Strict instance field

    // The Car creates its own Engine. They die together.
    Car() {
        this.engine = new Engine();
    }
}
```

## 5. Inheritance / Generalization (The "Is-A" Relationship)

Inheritance is a structural mechanism where a child class inherits all the traits (fields and methods) from a parent class. It allows you to reuse code and create a clear type hierarchy.

**Real-world Example:** A Dog is a type of Animal. It automatically inherits traits like breathing and eating.

**How it looks in Java:** Achieved directly using the `extends` keyword.

```java
class Animal {
    void eat() {
        System.out.println("Eating food...");
    }
}

// Dog gets the eat() method automatically
class Dog extends Animal {
    void bark() {
        System.out.println("Woof!");
    }
}
```

## 6. Realization (The "Implements" Relationship)

Realization is the relationship between an interface (which is just a contract or a blueprint) and the concrete class that actually does the work.

**Real-world Example:** A LaserPrinter realizes the Printable contract. The interface says what to do, the class defines how to do it.

**How it looks in Java:** Achieved directly using the `implements` keyword.

```java
interface Printable {
    void printJob(); // Just a blueprint contract
}

// LaserPrinter actually executes the blueprint
class LaserPrinter implements Printable {
    public void printJob() {
        System.out.println("Printing page using laser...");
    }
}
```