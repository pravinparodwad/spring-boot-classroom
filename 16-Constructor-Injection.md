# Spring Core – Constructor Injection (Interview-Ready Notes)

## 📌 What is Constructor Injection?
Constructor Injection is a type of **Dependency Injection (DI)** where the Spring IoC container:
- Uses a **parameterized constructor**
- Creates the **dependent object first**
- Injects it **during object creation**

> Injection happens **at construction time**, not after object creation.

---

## 📦 Types of Spring Containers
- **BeanFactory**
- **ApplicationContext** (extends BeanFactory)

---

## 🧠 Key Concept
If the Spring container uses a **parameterized constructor** to:
- Create a Spring bean
- Inject its dependencies  
➡️ This is called **Constructor Injection**

---

## 🔧 XML Configuration
Use `<constructor-arg>` inside `<bean>`

```xml
<bean id="wmg" class="com.ent.beans.WishMessageGenerator">
    <constructor-arg ref="dt"/>
</bean>

<bean id="dt" class="java.util.Date"/>
```

---

## 🧩 How Spring Decides Which Constructor to Use
- Number of `<constructor-arg>` tags = constructor parameter count
- 1 tag → 1-arg constructor
- 2 tags → 2-arg constructor

❌ Mismatch → Runtime Exception

---

## 🧪 Java Class Example

### Dependent Class
```java
public class WishMessageGenerator {
    private Date date;

    public WishMessageGenerator(Date date) {
        this.date = date;
    }

    public void generateMessage() {
        System.out.println("Date: " + date);
    }
}
```

---

## ⚙️ Execution Flow (Very Important 🔥)

### Setter Injection
1. Create target object
2. Create dependent object
3. Call setter

### Constructor Injection
1. Create dependent object
2. Pass it to constructor
3. Create target object

➡️ **Constructor Injection is faster**

---

## 🏎️ Performance Comparison

| Aspect | Constructor Injection | Setter Injection |
|------|----------------------|----------------|
| Speed | Faster ✅ | Slower |
| Immutability | Yes | No |
| Mandatory Dependency | Yes | Optional |
| Preferred | ✅ Yes | Sometimes |

---

## 🔍 Internal Working (Interview Gold ⭐)

### `getBean("wmg")` Flow
1. Check internal cache
2. Read `applicationContext.xml`
3. Detect constructor injection
4. Create dependent bean (`Date`)
5. Use Reflection API:
```java
Class.forName(...)
getDeclaredConstructors()
newInstance(dependency)
```
6. Store objects in cache
7. Return bean to client

---

## 🧠 Reflection Code (Conceptual)
```java
Class<?> clazz = Class.forName("WishMessageGenerator");
Constructor<?> ctor = clazz.getDeclaredConstructors()[0];
Object obj = ctor.newInstance(dateObj);
```

---

## ⚠️ Setter + Constructor Injection Together?
If **both are used on the same property**:

➡️ **Setter Injection overrides Constructor Injection**

Why?
- Constructor runs first
- Setter runs later

✅ Final value = Setter value

---

## 🔄 Multiple Beans from Same Class
```xml
<bean id="dt" class="java.util.Date"/>
<bean id="dt1" class="java.util.Date"/>
```

✔️ Same class  
✔️ Different objects  
✔️ Bean IDs must be unique

---

## 📝 Important Rules
- Use `ref` → inject objects
- Use `value` → inject primitives / Strings
- Bean ID = object reference name
- Constructor Injection → no setter required

---

## 🎯 Interview Questions

### Q1. Which is faster?
✅ Constructor Injection

### Q2. Which one is preferred?
✅ Constructor Injection (mandatory dependency)

### Q3. What happens if constructor count mismatches?
❌ Runtime Exception

### Q4. Which value wins: Setter or Constructor?
✅ Setter Injection

---

## ✅ When to Use Constructor Injection
- Mandatory dependencies
- Immutable objects
- Cleaner, safer design
- Recommended by Spring

---

## 🚀 Summary
- Constructor Injection injects dependencies **at object creation**
- Faster and safer than Setter Injection
- Uses Reflection API internally
- Setter Injection always overrides Constructor Injection if both exist

---

**✔ You are now interview-ready for Spring Constructor Injection**
