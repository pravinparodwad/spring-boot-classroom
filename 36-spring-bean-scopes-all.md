# Spring Bean Scopes - Interview Ready Notes

Spring Bean scopes control object lifecycle and sharing in the Spring IoC container. Default scope is **singleton**, but others like prototype, request, session, and application serve specific use cases in standalone and web applications.[1]

## Core Scopes Overview

Spring 5.x provides these key scopes:
- **Singleton** (default): One instance per container, stored in internal cache
- **Prototype**: New instance per `getBean()` call, no caching
- **Request**: One instance per HTTP request (web only)
- **Session**: One instance per user session/browser (web only)
- **Application**: One instance per web application (web only)

| Scope | # Objects | Usage Context | Storage |
|-------|-----------|---------------|---------|
| Singleton | 1 per container | DAO, Service, Controller | IoC cache[1] |
| Prototype | Unlimited | VO/DTO with changing state | None[1] |
| Request | 1 per request | Form data beans | HttpServletRequest[1] |
| Session | 1 per browser | Login credentials | HttpSession[1] |
| Application | 1 per app | Global counters | ServletContext[1] |

## Singleton Scope (Default)

**Key Concept**: Single shared instance across entire application lifecycle.

**When to use**:
- Classes with **no state** or **read-only state**
- **DAO, DataSource, Service, Controller** classes (effectively immutable)[1]
- Cache implementations with shareable state

```xml
<!-- applicationContext.xml -->
<bean id="dataSource" class="org.example.DataSource"/>
<!-- Default: scope="singleton" -->
```

**Benefits**: Memory efficient, thread-safe for stateless beans.[1]

## Prototype Scope

**Key Annotation**: `@Scope("prototype")`

**Behavior**: Creates **new object every `factory.getBean()` call**. No internal caching - **zero reusability**.[1]

**Demo Code**:
```java
ApplicationContext ctx = new ClassPathXmlApplicationContext("app.xml");
MessageGenerator mg1 = ctx.getBean("mg", MessageGenerator.class);  // New object
MessageGenerator mg2 = ctx.getBean("mg", MessageGenerator.class);  // NEW object!
System.out.println(mg1.hashCode() != mg2.hashCode());  // true[file:1]
```

**When to use**:
- **VO/DTO classes** holding changing state (request-to-request)
- Multiple concurrent users (300+ registrations → 300 VO objects needed)[1]
- **Stateful objects** where singleton would cause data override

**XML Config**:
```xml
<bean id="customerVO" class="CustomerVO" scope="prototype"/>
```

**Vs Prototype Design Pattern**: Spring creates via **reflection** (constructor executes), not cloning.[1]

## Web Scopes (Request, Session, Application)

**Request Scope** (`scope="request"`):
- **1 object per HTTP request**
- Shared across all components processing **same request** (Servlet → JSP → etc.)
- **Form data beans** recommended over prototype for request lifecycle[1]

**Session Scope** (`scope="session"`):
- **1 object per browser session**
- Login credentials (username, password, user profile)[1]
- Different browsers = different objects

**Application Scope** (`scope="application"`):
- **1 object per entire web app**
- Global counters (requestCount, userCount, daysCount)[1]
- Stored in **ServletContext** (vs singleton's IoC cache)

## Interview Tricky Questions

### Q1: Singleton Java Class + Prototype Scope?
**Answer**: **Singleton behavior breaks**! Spring uses **reflection** to bypass private constructor.[1]

**Solutions**:
1. **Static factory method**:
```xml
<bean id="printer" class="Printer" scope="prototype" 
      factory-method="getInstance"/>
```
   - Printer's instance acts as "mini-cache"[1]

2. **Reflection-proof singleton** (enum/singleton with validation)

### Q2: Prototype Bean Acting Like Singleton?
**Answer**: Use **Singleton Java class + static factory method + prototype scope**. Factory caches internally.[1]

### Q3: Constructor Exceptions?
```
Object created → NO constructor: Cloning, Deserialization
Constructor executes → NO object: Abstract class (subclass instantiation)
```
**Pro tip**: Reverse question → "When did you study Design Patterns?"[1]

## Real-World Layered App Scopes

```
Controller → Service → DAO     : **singleton** (stateless)
                          ↓
VO → DTO → VO             : **prototype** (stateful per user)[file:1]
```

**300 concurrent users**:
- Singleton VO: **Last user overwrites all** → Disaster!
- Prototype VO: **300 separate objects** → Correct![1]

## Quick Revision Table

| Scenario | Recommended Scope |
|----------|-------------------|
| DataSource/DAO/Service/Controller | **singleton**[1] |
| VO/DTO (form data, user state) | **prototype**[1] |
| Per-request form beans | **request** |
| Login user info | **session** |
| App-wide counters | **application** |

