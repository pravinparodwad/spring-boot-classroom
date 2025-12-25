## Spring BeanFactory: DefaultListableBeanFactory & Design Patterns

This lecture covers limitations of XmlBeanFactory, advantages of DefaultListableBeanFactory with XmlBeanDefinitionReader, and introduces Factory design pattern in Spring context.[1]

## XmlBeanFactory Limitations

XmlBeanFactory (deprecated since Spring 3.1) has key restrictions making it unsuitable for advanced use.

- **Deprecated Class**: Removed in favor of DefaultListableBeanFactory + XmlBeanDefinitionReader for better XML parsing and multiple file support.[1]
- **Single Config File**: Cannot load multiple Spring bean XML files simultaneously.[1]
- **Resource Objects Required**: Expects FileSystemResource or ClassPathResource, not simple string paths.[1]
- **Internal Delegation**: Forwards beans to DefaultListableBeanFactory anyway, so use it directly.[1]

## DefaultListableBeanFactory Advantages

**Key Concept**: Core IOC container implementation; extensible via readers for bean definitions.

- **Not Deprecated**: Future-proof for production use.[1]
- **Multiple XML Support**: Varargs `loadBeanDefinitions(String... locations)` accepts comma-separated paths.[1]
- **String Paths**: Pass config locations directly as strings (e.g., "com/ent/cfc/applicationContext.xml").[1]
- **Direct Bean Registration**: Java classes become beans without intermediate classes.[1]

### Creation Code Snippet
```java
DefaultListableBeanFactory factory = new DefaultListableBeanFactory();
XmlBeanDefinitionReader reader = new XmlBeanDefinitionReader(factory);
reader.loadBeanDefinitions("com/ent/cfc/applicationContext.xml");  // Relative to src/main/java
// Now factory.getBean("beanId") works with loaded beans
```
**Memory Flow**: Reader holds factory reference → loads XML metadata → factory accesses indirectly (hostel analogy: joined hostel gets food access).[1]

## Factory Design Pattern in Spring

**Core Interview Point**: Spring's BeanFactory/ApplicationContext implement **Factory Pattern** via `getBean()` - abstracts object creation, dependencies, and initialization.

### Factory Pattern Definition
Returns one of several **related classes** (common superclass/interface) based on input data, hiding creation complexity.[1]

- **Related Classes**: Share superclass (e.g., Person) or interface (e.g., Connection).[1]
- **Abstraction**: Client supplies data (bean ID); factory handles instantiation, dependencies, init.[1]
- **Factory Method**: Static method with common return type (e.g., `public static Person getPerson(String type)`).[1]

### Code Snippet: PersonFactory Example
```java
public class PersonFactory {
    public static Person getPerson(String type) {
        if ("EMP".equalsIgnoreCase(type)) return new Employee();
        else if ("CUST".equalsIgnoreCase(type)) return new Customer();
        else if ("STUD".equalsIgnoreCase(type)) return new Student();
        else throw new IllegalArgumentException("Invalid person type");
    }
}
// Usage: Person p = PersonFactory.getPerson("EMP");  // No creation details exposed
```
**Why Static?** Avoids factory object creation; client wants Person, not factory instance.[1]

### Real-World Spring Examples
| Example | Factory Method | Returns | Abstraction Provided [1] |
|---------|----------------|---------|-------------------------------|
| `DriverManager.getConnection(url, user, pwd)` | Varies by DB (Oracle/MySQL) | `Connection` impl | Driver loading, pooling hidden |
| `BeanFactory.getBean("id")` | Bean name/type | Bean instance + deps | Class loading, DI, init hidden |
| `Connection.createStatement()` | DB-specific | `Statement` impl | Vendor-specific creation |
| Car Factory | Model number | Car object | Parts assembly, integration |

**Interview Tip**: "Spring BeanFactory is a Factory Pattern implementation - `getBean()` creates related bean objects (common interface) based on config data, injecting dependencies transparently."[1]

## Design Patterns Overview

**Analogy**: Languages/frameworks = medicine; Design Patterns = antibiotics (boost effectiveness, solve recurring issues like memory/performance/tight-coupling).[1]

- **Gang of Four (GoF)**: 23 patterns for standalone apps (Singleton, Factory, Strategy in Spring).[1]
- **J2EE Patterns**: Front Controller, DAO, View Helper for web apps.[1]
- **Microservices**: Saga, etc. (learn after Spring Boot).[1]

**Spring Focus**: Factory (BeanFactory), Strategy (upcoming).[1]

## README.md Content (Copy & Save as `spring-beanfactory-notes.md`)

```
# Spring Boot: DefaultListableBeanFactory & Factory Pattern Notes

## Quick Interview Answers
- **Why DefaultListableBeanFactory over XmlBeanFactory?** Not deprecated, multi-XML, string paths [file:1]
- **BeanFactory = Factory Pattern?** Yes, `getBean()` abstracts creation/DI [file:1]
- **Factory Method Signature?** `static CommonType getX(String input)` [file:1]

## Core Code
```
// IOC Container Setup
DefaultListableBeanFactory factory = new DefaultListableBeanFactory();
new XmlBeanDefinitionReader(factory).loadBeanDefinitions("beans.xml");
```

## Key Takeaways
- Avoid XmlBeanFactory (deprecated Spring 3.1) [file:1]
- Factory Pattern: Hides "how" for related objects [file:1]
- Spring = Factory Pattern in action daily [file:1]

Lecture: NTSPBMS615 (Sept) | Prepared: Dec 2025
```
