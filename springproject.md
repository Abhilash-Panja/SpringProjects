Here’s a **beginner-friendly**, elegantly structured guide designed as a GitHub README. This document progressively builds the **Online Bookstore API** project while explaining concepts in simple terms.

---

# 📚 Online Bookstore API

_A Spring Boot Learning Journey for Beginners_

![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.x-green)  
![Project Progress](https://img.shields.io/badge/Phase-1%20of%204-blue)

---

## 🎯 **Project Overview**

Build a REST API for an online bookstore, starting with basic CRUD operations and evolving into microservices with security, testing, and resilience.

**Learning Outcomes**:

- ✅ Build RESTful APIs with Spring Boot.
- ✅ Secure APIs with JWT and Spring Security.
- ✅ Split a monolith into microservices.
- ✅ Handle failures with circuit breakers.

---

## 🚀 **Phase 1: Basics**

_Create a monolithic Spring Boot app with CRUD operations and H2 database._

---

### 📂 **Task 1: Create a Spring Boot Project**

**What You'll Learn**: Setting up a Spring Boot project.

#### Steps:

1. Go to **[Spring Initializr](https://start.spring.io)**.
2. Configure:
   - **Project**: Maven
   - **Language**: Java
   - **Dependencies**: `Spring Web`, `Spring Data JPA`, `H2 Database`, `Spring Boot DevTools`.
3. Click **Generate** and open the project in your IDE (e.g., IntelliJ).

#### Verify Success:

- Run `src/main/java/com/example/bookstore/BookstoreApplication.java`.
- If the app starts on `http://localhost:8080`, you’re ready!

---

### 📘 **Task 2: Create a Book Entity**

**What You'll Learn**: JPA and database modeling.

#### Steps:

1. Create a `Book` class in the `model` package:
   ```java
   @Entity
   public class Book {
       @Id
       @GeneratedValue(strategy = GenerationType.IDENTITY)
       private Long id;
       private String title;
       private String author;
       private Double price;
       // Add getters and setters here
   }
   ```
2. Create a repository interface:
   ```java
   public interface BookRepository extends JpaRepository<Book, Long> {}
   ```

#### Verify Success:

- Visit the H2 console at `http://localhost:8080/h2-console`.
- Use JDBC URL: `jdbc:h2:mem:testdb` (no password).

---

### 🔄 **Task 3: Build REST Endpoints**

**What You'll Learn**: REST API design with Spring MVC.

#### Steps:

1. Create a `BookController` class:

   ```java
   @RestController
   @RequestMapping("/books")
   public class BookController {
       @Autowired
       private BookRepository bookRepository;

       // Get all books
       @GetMapping
       public List<Book> getAllBooks() {
           return bookRepository.findAll();
       }

       // Add a new book
       @PostMapping
       public Book addBook(@RequestBody Book book) {
           return bookRepository.save(book);
       }
   }
   ```

#### Verify Success:

- Use **Postman** to send a `POST` request to `http://localhost:8080/books` with JSON:
  ```json
  { "title": "The Hobbit", "author": "J.R.R. Tolkien", "price": 15.99 }
  ```
- Send a `GET` request to `/books` to see the saved book.

---

### ⚙️ **Task 4: Configure Profiles**

**What You'll Learn**: Environment-specific configurations.

#### Steps:

1. Create `application-dev.properties` (for H2):
   ```properties
   spring.datasource.url=jdbc:h2:mem:testdb
   spring.h2.console.enabled=true
   ```
2. Create `application-prod.properties` (for PostgreSQL):
   ```properties
   spring.datasource.url=jdbc:postgresql://localhost:5432/bookstore
   spring.datasource.username=postgres
   spring.datasource.password=your_password
   spring.jpa.hibernate.ddl-auto=update
   ```

#### Verify Success:

- Run the app with `--spring.profiles.active=prod` (ensure PostgreSQL is installed).

---

## 🌟 **Phase 2: Intermediate**

_Add security, exception handling, and testing._

---

### 🛡️ **Task 1: Secure the API**

**What You'll Learn**: Basic authentication with Spring Security.

#### Steps:

1. Add the `spring-boot-starter-security` dependency.
2. Create a `SecurityConfig` class:
   ```java
   @Configuration
   public class SecurityConfig {
       @Bean
       public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
           http
               .authorizeRequests()
               .antMatchers("/admin/**").authenticated()
               .anyRequest().permitAll()
               .and()
               .httpBasic();
           return http.build();
       }
   }
   ```

#### Verify Success:

- Try accessing `/admin` (create a dummy endpoint). You’ll be prompted to log in (default username: `user`, password: check the console).

---

_(Phases 3 and 4 are simplified here for brevity. Expand as needed!)_

---

## 📂 **Project Structure**

```
bookstore-api/
├── book-service/          # Monolithic app (Phase 1-2)
├── order-service/         # Microservice (Phase 3)
├── eureka-server/         # Service discovery (Phase 3)
└── docker-compose.yml     # Deployment (Phase 4)
```

---

## 🛠️ **Tools & Resources**

- **Postman**: Test APIs.
- **H2 Console**: `http://localhost:8080/h2-console`.
- **Spring Docs**: [Spring Boot Guides](https://spring.io/guides).

---

## 🚧 **Next Steps**

1. Add JWT authentication (Phase 2).
2. Split the app into microservices (Phase 3).
3. Deploy with Docker (Phase 4).

---

**Happy Coding!** 🌟  
Let us know if you get stuck – we’re here to help! 💬

---

This README-style guide can be expanded for each phase. Let me know if you’d like me to flesh out **Phases 3 and 4** in the same format! 😊
