## Spring Factory Pattern Implementation

This lecture demonstrates **Factory Pattern** through a Car example, showing problem-solution approach and linking to Spring BeanFactory. Core concept: Factory abstracts object creation for related classes.[1]

## Problem Without Factory Pattern

**Client Burden**: Multiple clients must know class hierarchy, constructors, and dependencies.

### Car Hierarchy
```java
public abstract class Car {
    public abstract void drive();
}

public class BudgetCar extends Car {
    private String registrationNumber;
    public BudgetCar(String regNo) { this.registrationNumber = regNo; }
    public void drive() { System.out.println("Driving budget car"); }
}

public class LuxuryCar extends Car {
    private String registrationNumber;
    public LuxuryCar(String regNo) { this.registrationNumber = regNo; }
    public void drive() { System.out.println("Driving luxury car"); }
}

public class SportsCar extends Car {
    private String registrationNumber;
    public SportsCar(String regNo) { this.registrationNumber = regNo; }
    public void drive() { System.out.println("Driving sports car"); }
}
```

### Client Code (Problematic)
```java
// ProfessionalCustomer
Car car = new BudgetCar("TS09EN5656");
car.drive();  // Knows: subclass, constructor params, hierarchy

// YouthCustomer  
Car car = new SportsCar("GS10KK5656");
car.drive();  // Knows: subclass, constructor params, hierarchy

// BusinessmanCustomer
Car car = new LuxuryCar("TS11AB5151");
car.drive();  // Knows: subclass, constructor params, hierarchy
```
**Issues**:
- Clients know **exact class names** and **constructors**
- No abstraction of creation process
- Complex objects (Spring beans, JDBC connections) become impossible[1]

## Factory Pattern Solution

**CoreFactory** provides abstraction via `createCar()` method.

### Factory Implementation
```java
public class CoreFactory {
    public static Car createCar(String type, String registrationNumber) {
        if ("SPORTS".equalsIgnoreCase(type))
            return new SportsCar(registrationNumber);
        else if ("BUDGET".equalsIgnoreCase(type))
            return new BudgetCar(registrationNumber);
        else if ("LUXURY".equalsIgnoreCase(type))
            return new LuxuryCar(registrationNumber);
        else
            throw new IllegalArgumentException("Invalid car type");
    }
}
```

### Simplified Client Code
```java
// YouthCustomer (SIMPLIFIED)
Car car = CoreFactory.createCar("SPORTS", "TS08EN0068");
car.drive();  // "Driving sports car"

// ProfessionalCustomer
Car car = CoreFactory.createCar("BUDGET", "TS09EN5656");
car.drive();  // "Driving budget car"

// BusinessmanCustomer
Car car = CoreFactory.createCar("LUXURY", "TS11AB5151");
car.drive();  // "Driving luxury car"
```
**Benefits**:
- **Abstraction**: Clients supply `type` + `regNo`, factory handles rest
- **Common Return Type**: `Car` (superclass/interface)
- **Static Method**: No factory instance needed[1]

## Factory Method Characteristics

**Interview Key**: Factory method in Factory Pattern has specific traits.

| Characteristic | Details [1] |
|----------------|------------------|
| **Return Type** | Common superclass/interface (`Car`) |
| **Parameters** | Data for decision-making (`type`, `regNo`) |
| **Logic** | `if-else` based on input → returns **related class** object |
| **Static** | Usually static (client wants `Car`, not `CoreFactory`) |
| **Purpose** | Hides creation complexity, dependencies, initialization |

## Spring Integration

**BeanFactory = Factory Pattern**:
```java
BeanFactory factory = new XmlBeanFactory(...);
Date dt = factory.getBean("dt", Date.class);           // Date object
WishMessageGenerator wmg = factory.getBean("wmg");     // WMG + Date injected
```
- `getBean()` = Factory method
- Returns **bean objects** (related via config)
- Handles **dependencies automatically** (Date → WMG)[1]

**Real-world Examples**:
- `DriverManager.getConnection()` → JDBC Connection impl
- `Connection.createStatement()` → Statement impl
- **Spring**: `getBean()` → Bean + dependencies[1]

## Strategy Pattern Introduction

**Definition**: Makes target/dependent classes **loosely coupled interchangeable parts**.

### 3 Core Principles
1. **Favor Composition over Inheritance** (`has-a` vs `is-a`)
2. **Code to Interfaces** (not concrete classes)
3. **Open for Extension, Closed for Modification** (OCP)[1]

**Analogy**: Vehicle ↔ Engine (change Diesel/Petrol/CNG without modifying Vehicle)[1]

## README.md (Download Ready)

```markdown
# Spring Factory Pattern Notes (NTSPBMS615)

## 🚀 Interview Quick Answers
```
Q: What is Factory Pattern?
A: Returns related class objects (common interface) based on input, hiding creation logic.

Q: Spring BeanFactory example?
A: `getBean()` = Factory method → creates beans + injects dependencies transparently.

Q: Factory Method traits?
A: Static, common return type, if-else logic, abstracts object creation.
```

## 💻 Core Code
```
// Factory
public static Car createCar(String type, String regNo) {
    if("SPORTS".equalsIgnoreCase(type)) return new SportsCar(regNo);
    // ... other types
}

// Client
Car car = CoreFactory.createCar("BUDGET", "TS09EN5656");
car.drive();
```

## 🎯 Key Takeaways
- **Problem**: Clients know class hierarchy/constructors [file:2]
- **Solution**: Factory abstracts creation (`type` → object) [file:2]
- **Spring**: BeanFactory.getBean() = Factory Pattern daily [file:2]
- **Next**: Strategy Pattern (loose coupling via composition) [file:2]

*Lecture: Factory Pattern (Sept 23, 2021) | Interview-Ready*
```
