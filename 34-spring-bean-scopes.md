# Spring Bean Scopes – Deep Dive (Interview-Ready Notes)

---

## 1. What Is a Spring Bean Scope?

- **Bean Scope** defines the **visibility and lifecycle** of a Spring bean object
- Spring IoC container:
  - Creates beans
  - Manages lifecycle
  - **Controls how many objects are created and reused**

> Scope = how many objects + how long they live + where they are visible

---

## 2. Evolution of Bean Scopes

- **Spring 1.x**: singleton, prototype
- **Spring 2.x**: singleton (default), prototype, request, session, global-session
- **Spring 3.x – 5.x**: application, websocket (web scopes)

📌 Default scope in all versions → **singleton**

---

## 3. Singleton Scope (Default) ⭐⭐⭐

- One object **per Bean ID**
- Object stored in **IoC container internal cache**
- Same object reused across multiple `getBean()` calls

```xml
<bean id="wmg" class="com.nt.beans.WishMessageGenerator"/>
```

```java
WishMessageGenerator g1 = factory.getBean("wmg", WishMessageGenerator.class);
WishMessageGenerator g2 = factory.getBean("wmg", WishMessageGenerator.class);
System.out.println(g1 == g2); // true
```

---

## 4. Internal Cache – Core Mechanism ⭐

- Implemented as a **Map**
- Key → Bean ID
- Value → Object reference
- Enables **reusability & performance**

---

## 5. Singleton Scope ≠ Singleton Java Class 🚨

| Aspect | Singleton Java Class | Singleton Scope |
|------|--------------------|---------------|
| Restriction | Class-level | Container-level |
| Object count | One in JVM | One per Bean ID |
| Cache-based | ❌ | ✅ |

📌 Spring does NOT convert class into singleton Java class

---

## 6. Proof: Same Class + Different Bean IDs

```xml
<bean id="wmg1" class="com.nt.beans.WishMessageGenerator"/>
<bean id="wmg2" class="com.nt.beans.WishMessageGenerator"/>
```

```java
System.out.println(
 factory.getBean("wmg1") == factory.getBean("wmg2")
); // false
```

✔ Singleton is **Bean-ID based**

---

## 7. Singleton Java Class as Spring Bean – Issue ⚠️

- Spring uses **Reflection API**
- Can access private constructors
- Breaks singleton Java class if configured multiple times

---

## 8. Solution: Factory Method Bean Instantiation ⭐⭐⭐

```xml
<bean id="p1" class="com.nt.beans.Printer" factory-method="getInstance"/>
<bean id="p2" class="com.nt.beans.Printer" factory-method="getInstance"/>
```

✔ Same object returned  
✔ Internal cache stores duplicate references  

---

## 9. Web Scopes (High Level)

- `request`
- `session`
- `application`
- `websocket`

⚠️ Only for web applications

---

## 10. When to Use Singleton Scope?

✔ Stateless beans  
✔ Service / DAO layers  
✔ Shared read-only data  

---

## 11. Interview One-Liners 🎯

- Default scope is singleton
- Singleton scope is per Bean ID
- Singleton Java class ≠ Spring singleton
- Internal cache enables reuse
- Factory-method preserves singleton

---

## 12. Final Summary ✅

- Spring singleton = container behavior
- Java singleton = class restriction
- Understanding cache is key
- Very common interview topic

---

✔ Interview-ready on **Spring Bean Scopes**
