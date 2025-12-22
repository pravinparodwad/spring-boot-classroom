Spring Framework & Spring Boot – Core Concepts (Interview-Ready Notes)

⸻

1. What is the Spring Framework?
	•	Spring is a non-invasive, lightweight Java framework used to build:
	•	Standalone applications
	•	Web applications
	•	Distributed & enterprise applications
	•	It simplifies Java/J2EE development by handling boilerplate code and infrastructure concerns.
	•	Developed by Rod Johnson under the company Interface21 (later Pivotal → VMware).

💡 Spring complements J2EE technologies; it does NOT replace them.

⸻

2. What is a Spring Bean?

Definition

A Spring Bean is a Java class whose object lifecycle is created, managed, and destroyed by the Spring Container.

Key Points
	•	Bean = Object managed by Spring
	•	Can be:
	•	User-defined classes
	•	Predefined classes
	•	Third-party classes

⸻

3. Spring Containers

Spring provides two core containers:

1️⃣ BeanFactory
	•	Basic container
	•	Lazy initialization
	•	Lightweight

2️⃣ ApplicationContext
	•	Advanced container (most commonly used)
	•	Supports:
	•	Eager initialization
	•	Event propagation
	•	Internationalization
	•	Annotation support

📌 Both containers manage bean lifecycle & dependencies.

⸻

4. What is a Container? (Conceptual Understanding)
	•	A container is a software program that manages a resource from birth to death.
	•	Responsibilities:
	•	Load class
	•	Create object
	•	Manage object
	•	Destroy object

Analogy

🐠 Aquarium Example
	•	Fish = Spring Bean
	•	Aquarium = Spring Container
	•	Aquarium takes care of everything needed for survival

⸻

5. Framework Types

🔴 Invasive Frameworks

Definition:
	•	Application classes are tightly coupled with framework APIs.
	•	Classes must:
	•	Extend framework classes
	•	Implement framework interfaces

Characteristics:
	•	Cannot run without framework libraries
	•	High dependency
	•	No POJO/POJI support

Examples:
	•	Struts
	•	Servlets (Servlet API)

📌 Like joining a company with a bond — hard to leave.

⸻

🟢 Non-Invasive Frameworks

Definition:
	•	Application classes are loosely coupled with framework APIs.
	•	Classes are plain Java objects (POJOs).

Characteristics:
	•	No mandatory inheritance or interface implementation
	•	Can move business logic between frameworks
	•	Supports POJO/POJI programming

Examples:
	•	Spring
	•	Spring Boot
	•	Hibernate
	•	JSF

📌 Like joining a company without a bond — freedom to move.

⸻

6. History & Motivation of Spring
	•	EJB 2.x was:
	•	Heavy
	•	Complex
	•	Container-dependent
	•	Rod Johnson opposed this design
	•	Authored book: “J2EE Development Without EJB”
	•	Created Spring as:
	•	Lightweight
	•	POJO-based
	•	Flexible

🏆 EJB 3.x later adopted Spring concepts.

⸻

7. Is Spring an Alternative To…?

❓ Is Spring an alternative to EJB?

❌ No
	•	EJB = Distributed component technology
	•	Spring = All-rounder framework

❓ Is Spring an alternative to Struts?

❌ No
	•	Struts = Web framework only
	•	Spring = Web + Standalone + Distributed

❓ Is Spring an alternative to J2EE?

❌ No
	•	Spring internally uses J2EE technologies
	•	It simplifies them

⸻

8. Spring vs Spring Boot

Spring Framework
	•	Avoids Java/J2EE boilerplate code
	•	Requires manual configuration

Spring Boot
	•	Built on top of Spring
	•	Avoids Spring configuration boilerplate
	•	Provides:
	•	Auto-configuration
	•	Embedded servers
	•	Opinionated defaults

📌 Spring Boot = Extension of Spring, not replacement.

⸻

9. Spring Modules Overview

Core Modules
	•	Core
	•	Beans
	•	Context
	•	AOP
	•	JDBC
	•	ORM
	•	Transactions
	•	MVC
	•	JMS
	•	Mail
	•	Security

Extension Modules
	•	Spring Security
	•	Spring Batch
	•	Spring Data JPA
	•	Spring Data MongoDB
	•	Spring Cloud
	•	Spring REST

📌 Spring is modular so you include only what you need.

⸻

10. Why Spring Is Modular?
	•	Not every project needs every module
	•	Avoids:
	•	Large JAR sizes
	•	Unnecessary APIs
	•	Enables:
	•	Lightweight applications
	•	Faster startup

Analogy:
	•	À-la-carte restaurant vs unlimited buffet

⸻

11. Spring Bean Lifecycle Management

Handled by Spring Container:
	1.	Load class
	2.	Create object
	3.	Manage object
	4.	Destroy object

@Component
public class OrderService {
    public OrderService() {
        System.out.println("Bean Created");
    }
}


⸻

12. Dependency Management (Dependency Injection)

Key Concept

Assigning a dependent bean to a target bean dynamically is called Dependency Management / Dependency Injection.

Terminology

Role	Meaning
Target Class	Uses another class’s service
Dependent Class	Provides service

Example
	•	Flipkart → Target
	•	DTDC → Dependent

@Component
class DTDC {
    void deliver() {}
}

@Component
class Flipkart {
    @Autowired
    private DTDC dtdc;
}

☕ Before DI: You make coffee yourself
☕ After DI: Coffee is ready for you

⸻

13. Spring Configuration Approaches

1️⃣ XML Configuration
2️⃣ XML + Annotations
3️⃣ Java-based Configuration
4️⃣ Spring Boot (Auto-Configuration)

✅ Modern projects use Spring Boot.

⸻

14. Industry & Interview Perspective
	•	Spring Boot + Microservices = High demand
	•	Used in:
	•	Cloud-native apps
	•	Enterprise systems
	•	Interviews focus mainly on:
	•	Spring Core
	•	Spring Boot
	•	Microservices

⸻

15. Key Interview One-Liners
	•	Spring is non-invasive and POJO-based
	•	Spring Boot reduces Spring boilerplate
	•	Dependency Injection reduces programmer burden
	•	Spring complements J2EE, it doesn’t replace it
	•	ApplicationContext is preferred over BeanFactory

⸻

✅ Use this README as a quick revision guide and interview preparation reference.