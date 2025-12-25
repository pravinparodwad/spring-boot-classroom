# Spring Core – Setter Injection vs Constructor Injection (Interview-Ready Notes)

---

## 1. Dependency Injection Recap

- **Dependency Injection (DI)** = assigning dependent objects to target objects by the **Spring IoC container**
- Spring supports:
  - **Constructor Injection**
  - **Setter Injection**

---

## 2. Constructor Injection

### What is Constructor Injection?
- Dependencies are injected **at object creation time**
- Uses **parameterized constructors**

```java
public class Employee {
    private int id;
    private String name;

    public Employee(int id, String name) {
        this.id = id;
        this.name = name;
    }
}
```

### XML Configuration
```xml
<bean id="emp" class="com.demo.Employee">
    <constructor-arg name="id" value="101"/>
    <constructor-arg name="name" value="Rajesh"/>
</bean>
```

---

## 3. Key Rule of Constructor Injection ⚠️

> **All parameters of the chosen constructor MUST participate in injection**

- 3‑param constructor → all 3 values required
- Missing even one → **Runtime Exception**

✔ Best when **all bean properties are mandatory**

---

## 4. Problem with Constructor Injection (Deep Interview Insight ⭐)

If a bean has **N properties** and you want:
- **Choice-based injection** (some optional, some mandatory)

You need:
- **N! (factorial) overloaded constructors**

### Example
- 4 properties → 4! = 24 constructors
- 10 properties → 10! ≈ **36 lakh constructors** ❌

➡️ **Not practical**

---

## 5. Setter Injection

### What is Setter Injection?
- Dependencies injected **after object creation**
- Uses setter methods

```java
public class Student {
    private String name;
    private String college;

    public void setName(String name) {
        this.name = name;
    }

    public void setCollege(String college) {
        this.college = college;
    }
}
```

### XML Configuration
```xml
<bean id="stud" class="com.demo.Student">
    <property name="name" value="Anil"/>
    <property name="college" value="CBIT"/>
</bean>
```

---

## 6. Key Advantage of Setter Injection ✅

- Allows **choice-based injection**
- Missing property → **NO error**
- Unconfigured values become:
  - `null` (objects)
  - `0` (primitives)

✔ Best when **properties are optional**

---

## 7. Constructor vs Setter Injection – Comparison Table

| Aspect | Constructor Injection | Setter Injection |
|------|----------------------|----------------|
| Mandatory properties | ✅ Best | ❌ No guarantee |
| Optional properties | ❌ Complex | ✅ Best |
| Object immutability | ✅ Yes | ❌ No |
| Error if missing | ✅ Yes | ❌ No |
| Overloaded methods | Many constructors | Few setters |

---

## 8. Mixed Injection (Best Practice ⭐)

> When **some properties are mandatory** and **some are optional**

✔ Use **Constructor Injection** for mandatory  
✔ Use **Setter Injection** for optional

```java
public class Customer {
    private int id;
    private String name;
    private String address;

    public Customer(int id, String name) {   // mandatory
        this.id = id;
        this.name = name;
    }

    public void setAddress(String address) { // optional
        this.address = address;
    }
}
```

---

## 9. Important Spring Rules (Interview Traps 🚨)

- If **both setter & constructor injection are used on SAME property**:
  - **Setter injection wins**
- Constructor Injection → uses **Reflection API**
- DI is meant for **technical inputs**, not end‑user inputs
  - Examples:
    - JDBC URL
    - Driver class
    - Service objects

---

## 10. Which Constructor Does Spring Use?

| Configuration | Constructor Used |
|-------------|----------------|
| No injection | Zero‑arg constructor |
| Setter injection only | Zero‑arg constructor |
| Constructor injection | Parameterized constructor |

⚠️ **Default constructor ≠ Zero‑arg constructor**
- Default = compiler generated
- Zero‑arg = explicitly written

---

## 11. Interview One‑Liners 🎯

- Constructor Injection is preferred for **mandatory dependencies**
- Setter Injection is preferred for **optional dependencies**
- Choice‑based injection is NOT practical with constructors
- Setter Injection gives flexibility
- Best practice = **Combination approach**

---

## 12. Final Conclusion (Must Remember ⭐)

✔ **All properties mandatory** → Constructor Injection  
✔ **All properties optional** → Setter Injection  
✔ **Few mandatory + few optional** → Constructor + Setter  

---

## ✅ Summary

- Constructor Injection enforces strict dependency rules
- Setter Injection provides flexibility
- Use the right approach based on **business requirement**
- This topic is a **core interview favorite**

---

✔ You are now **fully interview‑ready** on Setter vs Constructor Injection
