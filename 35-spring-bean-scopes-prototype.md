# Spring Bean Scopes – Prototype & Advanced Scopes (Interview-Ready Notes)

---

## 1. Recap: What Is a Spring Bean Scope?

- **Bean Scope** determines:
  - How many objects Spring creates
  - How long they live
  - Where they are accessible
  - Managed by **Spring IoC container**

---

## 2. Prototype Scope ⭐⭐⭐

### Definition
- A **new object is created for every `getBean()` call**
- No caching of objects

```xml
<bean id="wmg" class="com.nt.beans.WishMessageGenerator" scope="prototype"/>
```

```java
Object o1 = factory.getBean("wmg");
Object o2 = factory.getBean("wmg");

System.out.println(o1 == o2); // false
```

---

## 3. Key Characteristics of Prototype Scope

- ❌ No object reuse
- ❌ No internal cache
- ✔ Multiple objects per Bean ID
- ✔ Suitable for **stateful beans**

---

## 4. Lifecycle Management (Very Important 🚨)

| Scope | Who Manages Lifecycle |
|----|----------------------|
| Singleton | Spring container |
| Prototype | Developer |

📌 Spring:
- Calls constructor
- Performs dependency injection
- Calls `init-method`

❌ Spring does **NOT** call:
- `destroy-method`
- `@PreDestroy`

---

## 5. Why Destroy Method Is Not Called?

- Container **does not track prototype instances**
- No reference stored
- Garbage collection handled by JVM

---

## 6. Prototype vs Singleton – Comparison

| Aspect | Singleton | Prototype |
|-----|---------|----------|
| Objects per Bean ID | 1 | Many |
| Caching | Yes | No |
| Stateful beans | ❌ | ✔ |
| Destroy callback | ✔ | ❌ |

---

## 7. Memory Considerations ⚠️

- Excessive prototype beans can cause:
  - Memory leaks
  - GC overhead
  - Developer must manage object cleanup

---

## 8. Mixing Scopes – Real World Scenario ⭐

### Problem
- Singleton bean depends on Prototype bean
- Prototype injected **only once**

### Result
- Prototype behaves like singleton

---

## 9. Solution: Method Injection (`lookup-method`) ⭐⭐⭐

```xml
<bean id="controller" class="com.nt.beans.MyController">
    <lookup-method name="getWishMessageGenerator" bean="wmg"/>
</bean>

<bean id="wmg" class="com.nt.beans.WishMessageGenerator" scope="prototype"/>
```

✔ New prototype object returned each time  
✔ Solves scope mismatch problem  

---

## 10. Alternative Solution: `@Lookup` Annotation

```java
@Component
public abstract class MyController {

    @Lookup
    public abstract WishMessageGenerator getWishMessageGenerator();
}
```

✔ Cleaner  
✔ Annotation-based  
✔ Preferred in Spring Boot

---

## 11. Web-Aware Scopes (Overview)

| Scope | Description |
|----|------------|
| request | One object per HTTP request |
| session | One object per HTTP session |
| application | One per ServletContext |
| websocket | WebSocket lifecycle |

⚠️ Only applicable for **web applications**

---

## 12. Interview One-Liners 🎯

- Prototype scope creates **new object per request**
- Prototype beans are **not destroyed by Spring**
- Prototype is suitable for **stateful beans**
- Singleton + Prototype causes scope mismatch
- `@Lookup` solves scope mismatch

---

## 13. Common Interview Questions 💡

**Q: Does Spring manage prototype bean lifecycle fully?**  
👉 No, only creation & initialization.

**Q: How to clean prototype beans?**  
👉 Developer responsibility.

**Q: Which scope is default?**  
👉 Singleton.

---

## 14. Final Summary ✅

- Prototype = new object every time
- No caching, no destroy callback
- Must handle memory carefully
- Use lookup-method or `@Lookup`
- Critical topic for Spring interviews

---

✔ You are now **interview-ready on Prototype & Advanced Bean Scopes**
