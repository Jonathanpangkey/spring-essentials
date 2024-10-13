# Spring Framework Core

## Introduction : IoC and Bean

An **IOC (Inversion of Control) Container** in Spring is like a "manager" that handles objects (called beans) and their dependencies for you. Instead of creating objects manually in your code, Spring does it for you and injects them wherever needed. This way, the control of creating objects (which is usually done manually in traditional programming) is inverted to the container, hence the name "Inversion of Control."

### Traditional Way of Creating Dependencies
In traditional programming, you manually create objects and their dependencies. For example:

```java
class Car {
    Engine engine;

    public Car() {
        this.engine = new Engine(); // Manually creating the engine
    }
}
```

Here, the `Car` class is responsible for creating the `Engine` object. This can make the code harder to test and maintain because the `Car` is tightly coupled with the `Engine`.

### IOC Container Approach (Using Spring)
In Spring, the **IOC Container** takes care of creating and managing dependencies. You don't manually create the `Engine` object. Instead, you declare them, and Spring injects them automatically.

#### Example: Car and Engine using Spring IOC
1. **Create the `Engine` class:**

```java
import org.springframework.stereotype.Component;

@Component // Marks this class as a "bean" that Spring will manage
public class Engine {
    public void start() {
        System.out.println("Engine started!");
    }
}
```

2. **Create the `Car` class and inject the `Engine`:**

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component // Marks Car as a bean managed by Spring
public class Car {

    private Engine engine;

    @Autowired // Tells Spring to inject the Engine dependency
    public Car(Engine engine) {
        this.engine = engine;
    }

    public void drive() {
        engine.start();
        System.out.println("Car is driving!");
    }
}
```

3. **Main Application (Spring Boot Example):**

```java
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.Bean;

@SpringBootApplication
public class CarApplication {

    public static void main(String[] args) {
        SpringApplication.run(CarApplication.class, args);
    }

    @Bean
    public CommandLineRunner run(ApplicationContext context) {
        return args -> {
            Car car = context.getBean(Car.class); // Spring provides the Car bean
            car.drive(); // The car drives and the engine starts automatically
        };
    }
}
```

### What is a **Bean**?
A **bean** is simply an object that Spring manages. It's an instance of a class that is created and controlled by the Spring container.

- Beans are created by Spring.
- They are initialized, destroyed, and wired automatically based on configuration.
- In the example above, `Engine` and `Car` are beans.

### Relation Between IOC and Beans
The **IOC container** is responsible for managing beans. The container handles:
- Creating bean objects.
- Injecting dependencies between beans (like the `Engine` into the `Car`).
- Managing the lifecycle of these beans (from creation to destruction).

### Summary
- **Traditional way**: You create dependencies manually (e.g., creating `Engine` inside `Car`).
- **IOC container**: Spring manages the dependencies automatically using beans.
- **Bean**: An object managed by Spring.
  
The key benefit of this approach is that it makes your code more flexible, testable, and modular because the object creation is managed outside your business logic, making it easy to swap components without changing much code.

In Spring, the **IOC (Inversion of Control) Container** manages objects and their dependencies automatically. Instead of manually creating objects as in traditional programming, Spring creates and manages these objects (called **beans**) and injects their required dependencies into other classes. This approach makes the code more flexible, easier to test, and less dependent on hard-coded implementation details.

A **bean** is an object managed by Spring, and the IOC container is responsible for its lifecycle, from creation to destruction. This allows dependencies between objects (such as `Car` and `Engine`) to be injected automatically by Spring, making the code more modular and easier to maintain, compared to the traditional method of manually creating objects within the code.



## What is Spring Boot?

**Spring Boot** is an extension of the Spring framework designed to simplify the development process, especially for building stand-alone, production-ready applications with minimal configuration. While Spring provides a powerful set of features for building Java applications, it often requires significant configuration. Spring Boot eliminates much of this configuration overhead by offering sensible defaults, auto-configuration, and embedded servers.

### How is Spring Boot Different from Spring?

1. **Less or No Configuration**:
   - With normal **Spring**, you often have to write a lot of setup code (configuration) to tell your app how things work, like how to connect to a database or which components to use.
   - **Spring Boot** does much of this for you. It guesses what you need and sets it up automatically.

2. **Starter Packs**:
   - **Spring Boot starters** are like pre-packaged kits that include everything you need for certain tasks. For example, if you're building a web app, you can use `spring-boot-starter-web`, and it will pull in everything you need to run a web application (web server, MVC framework, etc.).
   
3. **Standalone App with Embedded Server**:
   - Normally with Spring, you would need to deploy your app on an external web server (like Tomcat). With Spring Boot, your app **runs on its own**, because it includes the server (like Tomcat) inside the app. You can just run your app with a single command (`java -jar app.jar`) without needing to install anything else.

4. **Production-Ready Features**:
   - **Spring Boot** includes things like health checks, metrics, and monitoring out of the box. These are useful to see if your app is running well and is ready to be used in production (the real world).

### What is Auto-Configuration?

One of the biggest things **Spring Boot** does is **auto-configuration**. 

**Auto-configuration** means Spring Boot automatically sets things up for you based on what it finds in your project. Let’s say you have a database library in your project — Spring Boot will see this and automatically configure your app to connect to a database without you having to write that setup code manually.

**Example of Auto-Configuration**:
- If Spring Boot detects a **database** library like MySQL or H2 in your project, it will set up a connection to that database automatically.
- If Spring Boot sees that you’re building a **web** application (because you added `spring-boot-starter-web`), it will configure an embedded web server for you, so you don’t have to.

Like when you add a dependency like H2 or MySQL to your Spring Boot project, Spring Boot will automatically try to set up the basic configuration for you. This is part of auto-configuration. However, for certain parts like the data source (e.g., the database URL, username, password), you still need to provide some details, especially if you're using an external database like MySQL.
This is why Spring Boot applications require very little setup to get started — it takes care of much of the configuration behind the scenes, so you can focus on building your app.

### Use Case: Why Use Spring Boot?

Let's say you're building a simple **To-Do list** web application. With **Spring Boot**:
1. You don't need to configure a web server, because Spring Boot will set up one for you.
2. You don't need to manually configure how to connect to a database. Spring Boot will automatically do this if it detects a database library.
3. You get a **production-ready** app with health checks and monitoring features built in.

### Simple Code Example

1. **Main Application**:
   Spring Boot starts your app with the `@SpringBootApplication` annotation. This tells Spring Boot to auto-configure everything.
   
```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class TodoApplication {
    public static void main(String[] args) {
        SpringApplication.run(TodoApplication.class, args);  // This starts the app with everything ready
    }
}
```

2. **Auto-Configuration in Action**:
   If you add `spring-boot-starter-web` and `spring-boot-starter-data-jpa` to your project dependencies, Spring Boot will:
   - Set up an **embedded web server**.
   - Configure database access automatically (no manual setup required).

3. **Sample REST API**:
   This code creates a simple API that you can use to manage your to-do tasks.

```java
import org.springframework.web.bind.annotation.*;
import java.util.ArrayList;
import java.util.List;

@RestController  // This tells Spring Boot that this is a REST controller
@RequestMapping("/todos")
public class TodoController {

    private List<String> todos = new ArrayList<>();

    @GetMapping  // Handles HTTP GET requests to "/todos"
    public List<String> getTodos() {
        return todos;  // Returns the list of to-do tasks
    }

    @PostMapping  // Handles HTTP POST requests to "/todos"
    public void addTodo(@RequestBody String task) {
        todos.add(task);  // Adds a new task to the list
    }
}
```

### What Spring Boot Offers as a Whole:
- **Easy Setup**: You don’t have to manually configure servers, databases, etc. Spring Boot auto-configures everything for you.
- **Embedded Server**: Your app runs on its own without needing an external web server (like Tomcat). This makes deployment easy.
- **Starter Packs**: Pre-made sets of tools for different tasks, like building web apps or connecting to databases.
- **Production-Ready**: Includes tools for monitoring, health checks, and metrics to ensure your app runs smoothly in production.

### Final Thoughts

Spring Boot makes building Java applications fast and easy by reducing the setup work. It’s perfect for quickly getting an app up and running with the ability to add more complex features later if needed. If you're creating a small web app, an API, or even a microservice, Spring Boot is a great choice because it simplifies many common development tasks.


## Spring Data Overview
**Spring Data** is a framework within the Spring ecosystem that provides simplified and consistent data access for various databases. It abstracts the complexities of interacting with databases, allowing you to focus on higher-level business logic rather than the details of data access. It offers support for both relational and non-relational databases, and allows you to use repositories, query methods, and CRUD operations easily.

### 1. JDBC (Java Database Connectivity)

**JDBC** is a low-level API in Java that allows direct interaction with relational databases like MySQL, PostgreSQL, and Oracle. It is used to send SQL queries and updates, retrieve data, and manage database connections.

#### How JDBC Works
- You manually write SQL queries.
- You need to handle result sets, connections, prepared statements, and exceptions.
- It requires significant boilerplate code for connecting, querying, and closing resources.

#### Example with JDBC:
Here’s a typical example of how JDBC works:
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

public class JdbcExample {
    public static void main(String[] args) {
        String url = "jdbc:postgresql://localhost:5432/mydb";
        String user = "user";
        String password = "password";

        try (Connection connection = DriverManager.getConnection(url, user, password);
             PreparedStatement statement = connection.prepareStatement("SELECT * FROM students")) {

            ResultSet resultSet = statement.executeQuery();
            while (resultSet.next()) {
                System.out.println(resultSet.getString("name"));
            }

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```


#### Challenges of JDBC:
- **Boilerplate Code**: You have to write a lot of code to manage connections, handle exceptions, and close resources.
- **Manual SQL Management**: You manually write SQL queries, making it harder to maintain as the project grows.

### 2. JPA (Java Persistence API)

**JPA** is a higher-level API that provides a way to map Java objects to database tables (object-relational mapping or ORM). It abstracts the SQL queries, allowing developers to interact with the database using Java objects and methods. JPA is often used in conjunction with **Hibernate**, which is a popular JPA implementation. So unlike JDBC that need you to write manual SQL queries, JPA uses Java methods to interact with databases.

#### How JPA Works:
- You define entities (Java classes) that map to database tables.
- SQL queries are abstracted, and you use methods (like `find()`, `save()`) to interact with the database.
- JPA manages the persistence of objects without manually writing SQL.

#### Example with JPA:
```java
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.Persistence;

@Entity
public class Student {
    @Id
    private Long id;
    private String name;

    // getters and setters
}

public class JpaExample {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("my-persistence-unit");
        EntityManager em = emf.createEntityManager();

        em.getTransaction().begin();
        Student student = em.find(Student.class, 1L);
        System.out.println(student.getName());
        em.getTransaction().commit();

        em.close();
        emf.close();
    }
}
```

#### Why Use JPA?
- **Less Boilerplate**: JPA removes a lot of the boilerplate code by handling database interactions automatically.
- **Object-Oriented**: You work with objects rather than raw SQL, making your code cleaner and easier to understand.
- **ORM**: It bridges the gap between object-oriented programming and relational databases.


### 3. Hibernate

**Hibernate** is a popular implementation of JPA and is one of the most widely used ORMs in Java development. It provides additional features on top of JPA, such as caching, dirty checking, and more robust query capabilities with HQL (Hibernate Query Language).

#### Key Features of Hibernate:
- **Automatic Table Creation**: Hibernate can automatically create database tables based on your entities.
- **Lazy Loading**: Data is loaded from the database only when it’s needed, improving performance.
- **Caching**: It offers built-in caching for more efficient data retrieval.

#### Example with Hibernate:
Using Hibernate is essentially the same as using JPA since Hibernate implements JPA.

```java
@Entity
public class Book {
    @Id
    private Long id;
    private String title;

    // Getters and Setters
}

public class HibernateExample {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("my-persistence-unit");
        EntityManager em = emf.createEntityManager();

        em.getTransaction().begin();
        Book book = em.find(Book.class, 1L);
        System.out.println(book.getTitle());
        em.getTransaction().commit();

        em.close();
        emf.close();
    }
}
```

### Difference Between JPA and Hibernate

- **JPA** is a specification, while **Hibernate** is an implementation of that specification. This means JPA defines how ORM should work in Java, but doesn’t provide the code to do it. Hibernate implements JPA, adding its own extra features.
- **JPA** provides a standard way to work with database entities, but **Hibernate** goes further by adding specific features like caching, better performance optimization, and extra query options.
  
#### Why Use Hibernate (over JPA)?
- **More Features**: Hibernate offers advanced features like caching and custom SQL handling beyond the JPA specification.
- **Widely Used**: It’s one of the most mature and widely adopted ORM frameworks.
- **Caching and Optimization**: Hibernate’s built-in caching can improve performance by reducing redundant database calls.


### Conclusion

- **JDBC** is useful when you need low-level control over database access and want to write raw SQL. However, it requires a lot of manual work.
- **JPA** abstracts away much of the manual work by mapping Java objects to database tables, providing a cleaner and more object-oriented way to handle database interactions.
- **Hibernate** extends JPA with additional features, offering more advanced optimizations, caching, and query capabilities.

**Spring Data** simplifies using both JDBC and JPA (via Hibernate) by providing an environment where you can focus more on your application's business logic, while it takes care of the complexities of data access. You choose the appropriate approach depending on your specific needs for control, performance, and ease of development.
