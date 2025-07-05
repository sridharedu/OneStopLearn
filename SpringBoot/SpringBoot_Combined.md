# SpringBoot Combined Notes

## 001.SpringBoot-introduction.md
🚀 **Introduction**
- → Spring Boot is an open-source, micro-framework by Pivotal.
- → Simplifies bootstrapping and development of new Spring applications.
- → Aims to create stand-alone, production-grade Spring-based applications.

✨ **Key Features**
- → **Stand-alone Applications:** Create executable JARs runnable with `java -jar`.
- → **Embedded Servers:** Includes Tomcat, Jetty, or Undertow; no WAR deployment needed.
- → **Auto-configuration:** Automatically configures Spring app based on classpath and project setup.
- → **Opinionated Setup:** Provides "starter" dependencies for simplified build config.
- → **No XML Configuration:** Favors convention over configuration and annotation-based config.
- → **Production-ready Features:** Offers metrics, health checks, externalized configuration.

💡 **Why Use Spring Boot?**
- → **Rapid Application Development:** Reduces boilerplate code and configuration.
- → **Increased Productivity:** Focus on business logic, less on infrastructure setup.
- → **Easy to Get Started:** Quick setup and running of Spring applications.
- → **Microservices Ready:** Well-suited for building microservices architectures.

## 002.SpringBoot-starters.md
📦 **Spring Boot Starters**
- → Convenient dependency descriptors for Spring and related technologies.
- → Provide a one-stop-shop for all necessary dependencies.

⚙️ **How Starters Work**
- → Essentially Maven/Gradle dependency POMs.
- → Contain a collection of other dependencies.
- → Including a starter brings in all necessary transitive dependencies for a feature.

🌟 **Common Spring Boot Starters**
- → `spring-boot-starter-web`: For web (RESTful) applications using Spring MVC; uses Tomcat.
- → `spring-boot-starter-data-jpa`: For Spring Data JPA with Hibernate.
- → `spring-boot-starter-security`: For Spring Security.
- → `spring-boot-starter-test`: For testing (JUnit, Mockito, Spring Test).
- → `spring-boot-starter-actuator`: For production-ready features (monitoring, management).
- → `spring-boot-starter-thymeleaf`: For web apps using Thymeleaf.
- → `spring-boot-starter-jdbc`: For JDBC with HikariCP.

✅ **Benefits of Using Starters**
- → **Simplified Dependency Management:** No worries about compatible library versions.
- → **Reduced Build Configuration:** Less XML/Groovy in build files.
- → **Faster Development:** Quick setup with common configurations.
- → **Consistency:** Promotes consistent dependency usage across projects.

## 003.SpringBoot-auto-configuration.md
⚙️ **Spring Boot Auto-configuration**
- → Automatically configures Spring application based on classpath JARs.
- → Example: If HSQLDB is on classpath, auto-configures an in-memory database if no manual config.

💡 **How Auto-configuration Works**
- → Uses `@EnableAutoConfiguration` (part of `@SpringBootApplication`).
- → Auto-configuration classes listed in `META-INF/spring.factories`.
- → "Opinionated" but can be overridden.

✅ **Key Principles**
- → **Conditional Configuration:** Uses `@Conditional` annotations (e.g., `@ConditionalOnClass`, `@ConditionalOnMissingBean`) to apply config only when conditions met.
- → **Convention over Configuration:** Sensible defaults based on common patterns.

🚫 **Disabling Auto-configuration**
- → Use `exclude` attribute of `@EnableAutoConfiguration` or `spring.autoconfigure.exclude` property.
```java
@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class})
public class MyApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}
```
```properties
spring.autoconfigure.exclude=org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration
```

## 004.SpringBoot-dependency-injection.md
💉 **Dependency Injection (DI)**
- → Core concept in Spring Framework.
- → Manages components (beans) and their dependencies.
- → Removes hard-coded dependencies, making code modular, testable, maintainable.

💡 **How DI Works in Spring Boot**
- → Spring uses an Inversion of Control (IoC) container.
- → Container manages object lifecycle and injects dependencies.

✨ **Types of Dependency Injection**
1.  **Constructor Injection (Recommended):**
    - → Dependencies provided via constructor.
    - → Ensures object created with all required dependencies.
    - → Makes object immutable and easier to test.
    ```java
    @Service
    public class MyService {
        private final MyRepository myRepository;

        public MyService(MyRepository myRepository) {
            this.myRepository = myRepository;
        }
    }
    ```
2.  **Setter Injection:**
    - → Dependencies provided via setter methods.
    - → Makes dependencies optional and mutable.
    ```java
    @Service
    public class AnotherService {
        private MyRepository myRepository;

        @Autowired
        public void setMyRepository(MyRepository myRepository) {
            this.myRepository = myRepository;
        }
    }
    ```
3.  **Field Injection (Least Recommended):**
    - → Dependencies injected directly into fields using `@Autowired`.
    - → Convenient but makes testing difficult without Spring container.
    ```java
    @Service
    public class YetAnotherService {
        @Autowired
        private MyRepository myRepository;
    }
    ```

📌 **`@Autowired` Annotation**
- → Marks constructor, setter, or field for autowiring.
- → Spring finds matching bean and injects it.

🏷️ **`@Qualifier` Annotation**
- → Used with `@Autowired` to specify a specific bean when multiple of same type exist.
```java
@Service
public class OrderService {
    @Autowired
    @Qualifier("primaryOrderRepository")
    private OrderRepository orderRepository;
}
```

## 005.SpringBoot-rest-controllers.md
🌐 **Spring Boot REST Controllers**
- → Builds RESTful web services using Spring MVC.
- → REST is an architectural style for distributed hypermedia systems.
- → RESTful web services built on HTTP protocol.

✨ **Key Annotations**
- → **`@RestController`**: Combines `@Controller` and `@ResponseBody`; returns data directly.
    ```java
    @RestController
    @RequestMapping("/api/products")
    public class ProductController {
        // ...
    }
    ```
- → **`@RequestMapping`**: Maps web requests to handler methods/classes.
- → **`@GetMapping`**: Shortcut for `GET` requests.
- → **`@PostMapping`**: Shortcut for `POST` requests.
- → **`@PutMapping`**: Shortcut for `PUT` requests.
- → **`@DeleteMapping`**: Shortcut for `DELETE` requests.
- → **`@RequestBody`**: Binds HTTP request body to method parameter.
- → **`@ResponseBody`**: (Implicit in `@RestController`) Binds return value to response body.
- → **`@PathVariable`**: Extracts values from URI path.
- → **`@RequestParam`**: Extracts values from query string.

📝 **Example REST Controller**
```java
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;
import java.util.Optional;

@RestController
@RequestMapping("/api/products")
public class ProductController {

    private final ProductService productService;

    public ProductController(ProductService productService) {
        this.productService = productService;
    }

    @GetMapping
    public List<Product> getAllProducts() {
        return productService.findAll();
    }

    @GetMapping("/{id}")
    public ResponseEntity<Product> getProductById(@PathVariable Long id) {
        return productService.findById(id)
                .map(ResponseEntity::ok)
                .orElse(ResponseEntity.notFound().build());
    }

    @PostMapping
    public ResponseEntity<Product> createProduct(@RequestBody Product product) {
        Product savedProduct = productService.save(product);
        return ResponseEntity.status(HttpStatus.CREATED).body(savedProduct);
    }

    @PutMapping("/{id}")
    public ResponseEntity<Product> updateProduct(@PathVariable Long id, @RequestBody Product product) {
        return productService.findById(id)
                .map(existingProduct -> {
                    existingProduct.setName(product.getName());
                    existingProduct.setPrice(product.getPrice());
                    return ResponseEntity.ok(productService.save(existingProduct));
                })
                .orElse(ResponseEntity.notFound().build());
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteProduct(@PathVariable Long id) {
        if (productService.existsById(id)) {
            productService.deleteById(id);
            return ResponseEntity.noContent().build();
        } else {
            return ResponseEntity.notFound().build();
        }
    }
}
```

## 006.SpringBoot-data-jpa.md
💾 **Spring Boot Data JPA**
- → Part of Spring Data project.
- → Provides consistent, Spring-based programming model for data access.
- → Simplifies JPA-based data access layers.

✨ **Key Features**
- → **Repository Abstraction:** Reduces boilerplate for data access layers.
- → **Automatic Query Generation:** Queries generated from method names (e.g., `findByLastNameAndFirstName`).
- → **Custom Queries:** Supports `@Query` annotation.
- → **Pagination and Sorting:** Built-in support.
- → **Integration with Spring Boot:** Seamless auto-configuration for data sources and JPA providers.

🛠️ **Setting up Spring Data JPA**
1.  **Add `spring-boot-starter-data-jpa` dependency:**
    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    ```
2.  **Configure database in `application.properties` (e.g., H2):**
    ```properties
    spring.datasource.url=jdbc:h2:mem:testdb
    spring.datasource.driverClassName=org.h2.Driver
    spring.datasource.username=sa
    spring.datasource.password=
    spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
    spring.h2.console.enabled=true
    ```

👤 **Creating an Entity**
- → Lightweight domain object representing a database table.
```java
import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;

@Entity
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String firstName;
    private String lastName;
    private String email;

    // Getters and Setters
    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getFirstName() {
        return firstName;
    }

    public void setFirstName(String firstName) {
        this.firstName = firstName;
    }

    public String getLastName() {
        return lastName;
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }
}
```

🗄️ **Creating a Repository Interface**
- → Extend `JpaRepository` or `CrudRepository`.
```java
import org.springframework.data.jpa.repository.JpaRepository;
import java.util.List;

public interface UserRepository extends JpaRepository<User, Long> {
    List<User> findByLastName(String lastName);
    List<User> findByFirstNameAndLastName(String firstName, String lastName);
}
```

🔄 **Using the Repository in a Service**
```java
import org.springframework.stereotype.Service;
import java.util.List;
import java.util.Optional;

@Service
public class UserService {

    private final UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public List<User> findAllUsers() {
        return userRepository.findAll();
    }

    public Optional<User> findUserById(Long id) {
        return userRepository.findById(id);
    }

    public User saveUser(User user) {
        return userRepository.save(user);
    }

    public void deleteUserById(Long id) {
        userRepository.deleteById(id);
    }

    public List<User> findUsersByLastName(String lastName) {
        return userRepository.findByLastName(lastName);
    }
}
```

## 007.SpringBoot-security.md
🔒 **Spring Boot Security**
- → Powerful and customizable authentication/access-control framework.
- → De-facto standard for securing Spring-based applications.
- → Spring Boot simplifies integration with minimal configuration.

✨ **Key Features**
- → **Authentication:** Verifying user identity (e.g., username/password, OAuth2).
- → **Authorization:** Determining what an authenticated user can do (e.g., role-based access).
- → **Protection against Common Attacks:** Built-in CSRF, session fixation, clickjacking protection.
- → **Integration:** Seamless with Spring MVC, WebFlux, etc.

🛠️ **Setting up Spring Security**
1.  **Add `spring-boot-starter-security` dependency:**
    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>
    ```
    - → Auto-configures basic security, default login page, random password (on startup).

🔑 **Basic In-Memory Authentication**
- → For simple apps or development.
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests((requests) -> requests
                .requestMatchers("/public/**").permitAll()
                .requestMatchers("/admin/**").hasRole("ADMIN")
                .anyRequest().authenticated()
            )
            .formLogin((form) -> form
                .loginPage("/login")
                .permitAll()
            )
            .logout((logout) -> logout.permitAll());
        return http.build();
    }

    @Bean
    public UserDetailsService userDetailsService() {
        UserDetails user = User.withDefaultPasswordEncoder()
            .username("user")
            .password("password")
            .roles("USER")
            .build();
        UserDetails admin = User.withDefaultPasswordEncoder()
            .username("admin")
            .password("adminpass")
            .roles("ADMIN", "USER")
            .build();
        return new InMemoryUserDetailsManager(user, admin);
    }
}
```

🛡️ **Securing Endpoints**
- → `permitAll()`: Allows access to all users.
- → `authenticated()`: Requires user authentication.
- → `hasRole("ROLE_NAME")`: Requires specific role (Spring Security prefixes with `ROLE_`).
- → `hasAuthority("AUTHORITY_NAME")`: Requires specific authority.

🚪 **Custom Login Page**
- → Specify using `formLogin().loginPage("/login")`.

🚫 **CSRF Protection**
- → Provided by default.
- → Can be disabled for REST APIs not using session-based auth (e.g., JWT).
```java
// Inside securityFilterChain method
http.csrf().disable();
```
- → **Note:** Disable with caution; understand implications.

## 008.SpringBoot-actuator.md
📊 **Spring Boot Actuator**
- → Provides production-ready features for monitoring and managing applications.
- → Offers built-in endpoints for health, metrics, traffic, database state.

✨ **Key Features**
- → **Monitoring:** Insights into application health, metrics, environment.
- → **Management:** Dynamic management of features (e.g., log levels).
- → **Auditing:** Records security events.

🛠️ **Setting up Spring Boot Actuator**
1.  **Add `spring-boot-starter-actuator` dependency:**
    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>
    ```

📍 **Actuator Endpoints**
- → Accessible via HTTP or JMX.
- → **`/health`**: Application health information.
- → **`/info`**: Arbitrary application information.
- → **`/metrics`**: Metrics information.
- → **`/beans`**: List of all Spring beans.
- → **`/env`**: Exposes `ConfigurableEnvironment` properties.
- → **`/loggers`**: View and configure log levels at runtime.
- → **`/shutdown`**: Gracefully shuts down application (disabled by default).

✅ **Enabling and Exposing Endpoints**
- → By default, only `/health` and `/info` are exposed over HTTP.
- → Configure in `application.properties` or `application.yml`:
    ```properties
    # Expose all endpoints (not recommended for production without security)
    management.endpoints.web.exposure.include=*

    # Expose specific endpoints
    # management.endpoints.web.exposure.include=health,info,metrics

    # Disable specific endpoints
    # management.endpoints.web.web.exposure.exclude=shutdown
    ```

🔒 **Securing Actuator Endpoints**
- → Crucial to secure, especially in production.
- → Use Spring Security to protect:
    ```java
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;
    import org.springframework.security.config.annotation.web.builders.HttpSecurity;
    import org.springframework.security.web.SecurityFilterChain;

    @Configuration
    public class ActuatorSecurityConfig {

        @Bean
        public SecurityFilterChain actuatorFilterChain(HttpSecurity http) throws Exception {
            http
                .securityMatcher("/actuator/**") // Apply security only to actuator endpoints
                .authorizeHttpRequests((requests) -> requests
                    .requestMatchers("/actuator/health", "/actuator/info").permitAll()
                    .requestMatchers("/actuator/**").hasRole("ADMIN")
                )
                .httpBasic(); // Or formLogin(), etc.
            return http.build();
        }
    }
    ```

➕ **Custom Endpoints**
- → Create custom endpoints with `@Endpoint` and `@ReadOperation`, `@WriteOperation`, or `@DeleteOperation`.

## 009.SpringBoot-testing.md
🧪 **Spring Boot Testing**
- → Provides utilities and annotations for effective testing.
- → Aims to simplify testing with auto-configured test slices.

✨ **Key Features**
- → **`spring-boot-starter-test`**: Provides JUnit 5, Spring Test, Mockito, AssertJ, Hamcrest, JSONassert, JsonPath.
- → **Test Slices:** Specialized annotations for testing specific layers (web, JPA) without full context load.
- → **Test Annotations:** Convenient annotations for test setup and execution.

📌 **Common Testing Annotations**
- → **`@SpringBootTest`**: Primary for integration tests; loads full Spring application context.
    ```java
    import org.junit.jupiter.api.Test;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.boot.test.context.SpringBootTest;

    import static org.assertj.core.api.Assertions.assertThat;

    @SpringBootTest
    public class MyServiceIntegrationTest {

        @Autowired
        private MyService myService;

        @Test
        void contextLoads() {
            assertThat(myService).isNotNull();
        }

        @Test
        void testMyServiceMethod() {
            // ... test logic
        }
    }
    ```
- → **`@WebMvcTest`**: For Spring MVC controllers; auto-configures MVC infra; limits scanned beans to web-relevant ones.
    ```java
    import org.junit.jupiter.api.Test;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
    import org.springframework.test.web.servlet.MockMvc;

    import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
    import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

    @WebMvcTest(MyController.class)
    public class MyControllerTest {

        @Autowired
        private MockMvc mockMvc;

        @Test
        void shouldReturnDefaultMessage() throws Exception {
            this.mockMvc.perform(get("/"))
                .andExpect(status().isOk());
        }
    }
    ```
- → **`@DataJpaTest`**: For JPA repositories; auto-configures in-memory DB; scans for `@Entity` and JPA repos.
    ```java
    import org.junit.jupiter.api.Test;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.boot.test.autoconfigure.orm.jpa.DataJpaTest;
    import static org.assertj.core.api.Assertions.assertThat;

    @DataJpaTest
    public class UserRepositoryTest {

        @Autowired
        private UserRepository userRepository;

        @Test
        void shouldFindUserByEmail() {
            // ... test logic
        }
    }
    ```
- → **`@MockBean`**: Adds mock objects to Spring context; useful for mocking external/difficult dependencies.
    ```java
    import org.junit.jupiter.api.Test;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.boot.test.context.SpringBootTest;
    import org.springframework.boot.test.mock.mockito.MockBean;

    import static org.mockito.Mockito.when;

    @SpringBootTest
    public class MyServiceTest {

        @Autowired
        private MyService myService;

        @MockBean
        private ExternalService externalService;

        @Test
        void testServiceWithMockedDependency() {
            when(externalService.getData()).thenReturn("Mocked Data");
            // ... test logic
        }
    }
    ```

🧪 **Unit Testing**
- → For pure unit tests, Spring Boot testing annotations are often not needed.
- → Use JUnit and Mockito directly for isolated class testing.
```java
import org.junit.jupiter.api.Test;
import org.mockito.Mockito;

import static org.junit.jupiter.api.Assertions.assertEquals;

public class MyUtilityTest {

    @Test
    void testAddMethod() {
        MyUtility myUtility = new MyUtility();
        assertEquals(5, myUtility.add(2, 3));
    }
}
```

## 010.SpringBoot-profiles.md
👤 **Spring Boot Profiles**
- → Segregates application configuration for different environments.
- → Allows different configurations (e.g., dev, prod, test) without code changes.

💡 **How Profiles Work**
- → Identified by names (e.g., `dev`, `prod`, `test`).
- → Define different beans, configurations, or properties per profile.
- → Only components of active profiles are registered with Spring context.

📝 **Defining Profile-Specific Properties**
- → Use `application-{profile}.properties` or `application-{profile}.yml`.

**`application.properties`:**
```properties
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.username=sa
spring.datasource.password=
```

**`application-prod.properties`:**
```properties
spring.datasource.url=jdbc:mysql://prod-db:3306/mydb
spring.datasource.username=produser
spring.datasource.password=prodpass
```

✅ **Activating Profiles**
1.  **Using `spring.profiles.active` property:**
    - → In `application.properties`:
        ```properties
        spring.profiles.active=dev
        ```
    - → As a command-line argument:
        ```bash
        java -jar myapp.jar --spring.profiles.active=prod
        ```
    - → As a system property:
        ```bash
        java -Dspring.profiles.active=prod -jar myapp.jar
        ```
2.  **Using Environment Variables:**
    ```bash
    export SPRING_PROFILES_ACTIVE=prod
    java -jar myapp.jar
    ```

🧩 **Profile-Specific Beans**
- → Mark Spring components with `@Profile` annotation.
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Profile;

@Configuration
public class DataSourceConfig {

    @Bean
    @Profile("dev")
    public DataSource devDataSource() {
        // Configure development data source
        return new EmbeddedDatabaseBuilder().setType(EmbeddedDatabaseType.H2).build();
    }

    @Bean
    @Profile("prod")
    public DataSource prodDataSource() {
        // Configure production data source
        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://prod-db:3306/mydb");
        dataSource.setUsername("produser");
        dataSource.setPassword("prodpass");
        return dataSource;
    }
}
```

🌟 **Benefits of Using Profiles**
- → **Environment-Specific Configuration:** Easy management for different environments.
- → **Reduced Errors:** Prevents accidental deployment of dev configs to prod.
- → **Flexibility:** Dynamic switching of configurations without code changes.
- → **Clean Codebase:** Keeps environment-specific logic out of main application code.

## 011.SpringBoot-dependency-injection-and-ioc.md
💉 **Dependency Injection (DI) & Inversion of Control (IoC)**
- → When developing software, code is organized across components (classes in Java).
- → Example: `ProductDao` performs database operations, `ProductController` (web layer) uses `ProductDao`.
- → Instead of `ProductController` creating `ProductDao` object, responsibility is delegated to external frameworks like Spring.
- → Spring dynamically creates `ProductDao` object at runtime and injects it into `ProductController`.
- → `ProductController` then uses `ProductDao` within its methods.
- → This process of external components/modules creating and injecting dependencies into required classes is Dependency Injection.
- → Developers can focus on business logic, not object creation.
- → Moving control of object creation from class to external component (like Spring) is Inversion of Control.
- → IoC is a design pattern where object creation control is inverted from application code to an external component.

## 012.SpringBoot-bean-scopes.md
🌱 **Spring Bean Scopes**
- → Two main Spring bean scopes: Singleton and Prototype.

🔄 **Singleton Scope**
- → Only one instance of a bean created for the entire IoC container.
- → Same instance injected wherever required.
- → Default scope.

🔄 **Prototype Scope**
- → Multiple instances of beans created and injected wherever required.

🤔 **Which to Use and Why?**
- → **Singleton:** Use if application needs statelessness or if classes/beans are stateless (e.g., controllers, DAOs).
- → **Prototype:** Use if beans have state.
- → If stateful beans are Singleton, data can be corrupted across multiple threads.
- → Prototype avoids multi-threading issues with state because each thread gets its own bean instance.

## 013.SpringBoot-prototype-in-singleton.md
🔄 **Prototype in Singleton**
- → When a prototype-scoped bean is injected into a singleton-scoped bean, the prototype bean is instantiated only once.
- → This happens during the singleton bean's creation, and the same instance of the prototype bean is reused for all subsequent calls.
- → To get a new instance of the prototype bean each time it's requested from within a singleton, you need to use a method injection or `ObjectFactory`/`Provider`.
- → **Method Injection (Lookup Method):** Define an abstract method in the singleton bean that returns the prototype bean. Spring will override this method to return a new instance each time.
```java
@Component
@Scope("singleton")
public abstract class SingletonBean {
    public abstract PrototypeBean getPrototypeBean();
}

@Component
@Scope("prototype")
public class PrototypeBean {
    // ...
}
```
- → **`ObjectFactory` or `Provider`:** Inject `ObjectFactory<PrototypeBean>` or `Provider<PrototypeBean>` into the singleton bean. Call `getObject()` or `get()` on it to retrieve a new instance.
```java
@Component
@Scope("singleton")
public class SingletonBean {
    @Autowired
    private ObjectFactory<PrototypeBean> prototypeBeanObjectFactory;

    public PrototypeBean getNewPrototypeBean() {
        return prototypeBeanObjectFactory.getObject();
    }
}

@Component
@Scope("prototype")
public class PrototypeBean {
    // ...
}
```

## 014.SpringBoot-http-scopes.md
🌐 **HTTP Scopes**
- → Different Spring bean HTTP context scopes: Request, Session, and Global.

➡️ **Request Scope**
- → A new bean instance created for every incoming HTTP request.

➡️ **Session Scope**
- → Only one bean instance used across a session.
- → Same bean instance used for multiple HTTP requests and responses within a given session.

➡️ **Global Scope (Global Session)**
- → Makes sense only if using Portlets within the application.
- → Applies across portlets, like a global session.
- → Same bean used across this global session or Portlets.

## 015.SpringBoot-problems-with-traditional-spring.md
❌ **Problems with Traditional Spring**
- → Used to face issues with extensive XML or Java-based configuration.
- → Cumbersome to maintain applications over time due to configuration.
- → Had to manually include all required modules/dependencies in Maven `pom.xml` or Gradle build file.
- → Ensured compatibility across modules (Core, MVC, DAO, ORM) when moving between versions.
- → Manually deployed applications to external web containers (Tomcat) or application servers (WebLogic, WebSphere).

## 016.SpringBoot-why-use-spring-boot.md
✨ **Auto-Configuration**
- → Eliminates extensive XML or Java-based configuration for Spring MVC, ORM (Hibernate, Spring Data JPA).
- → Automatically configures Dispatcher Servlet for web layer upon adding dependencies.
- → Creates data source and injects transaction manager for ORM layer (e.g., with Spring Data JPA) without manual code.

📦 **Starters & Dependency Management**
- → Solves module availability and version compatibility issues common in traditional Spring apps.
- → Spring Boot Starters (e.g., `spring-boot-starter-web`, `spring-boot-starter-data-jpa`) provide ready-to-use dependency sets.
- → Transitive pulling of required libraries simplifies dependency management.
- → Parent project handles version compatibility across all dependencies.

🚀 **Embedded Servers & Easy Deployment**
- → Includes embedded servlet containers (Tomcat by default, also Jetty, Undertow).
- → Allows direct execution of Spring Boot applications (e.g., "run as Spring Boot application") without external WAR deployment.
- → Simplifies creation and launching of applications.

📊 **Actuators for Monitoring**
- → Provides health checks and insights into application configuration.
- → Allows viewing internal mappings, information, and health metrics.
- → Easily enabled by adding a single dependency to `pom.xml` or `build.gradle`.
- → Facilitates health monitoring even in production environments.

📈 **Overall Benefits**
- → Increases development speed.
- → Simplifies application deployment.
- → Enhances application monitoring capabilities.

## 017.SpringBoot-what-is-springbootapplication.md
✨ **@SpringBootApplication**
- → Starting point of every Spring Boot application.
- → Top-level annotation composed of three other annotations:
  - → `@SpringBootConfiguration`
  - → `@EnableAutoConfiguration`
  - → `@ComponentScan`

⚙️ **@SpringBootConfiguration**
- → Indicates that the class can define Spring beans.
- → Allows defining methods that expose beans for dependency injection.

🚀 **@EnableAutoConfiguration**
- → Instructs Spring Boot to enable auto-configuration based on classpath dependencies.
- → Example: With `spring-boot-starter-web`, automatically configures Dispatcher Servlet.
- → Example: With `spring-boot-starter-data-jpa` and `MySQL Connector`, automatically creates a data source from `application.properties`.
- → Automatically creates and configures necessary beans based on detected dependencies.

🔍 **@ComponentScan**
- → Scans the current package and all its sub-packages.
- → Discovers Spring beans annotated with `@Component`, `@Service`, `@Repository`, etc.
- → Makes discovered beans available for dependency injection at runtime.

## 018.SpringBoot-what-is-springboottest.md
🧪 **@SpringBootTest**
- → Uses a Spring extension class to run tests instead of a typical JUnit runner.
- → The extension class searches for a class annotated with `@SpringBootApplication`.
- → Uses the `@SpringBootApplication` class to create the entire Spring context for the application.
- → Loads all beans and dependencies before tests are run.
- → Ensures the Spring context and beans are available for testing when `@Test` JUnit annotations are executed.

## 019.SpringBoot-what-is-springdatajpa.md
💾 **What is Spring Data JPA**
- → Simplifies the creation of data access layers for applications.
- → Eliminates boilerplate coding previously required with ORM tools like Hibernate (DAO interfaces, implementations, EntityManagers).
- → Developers create a JPA entity class and an interface extending Spring Data JPA repository interfaces.
- → Automatically handles CRUD operations and more against the database.
- → Internally manages `EntityManagerFactory` and `EntityManager`.
- → Supports finder methods, generating queries automatically from abstract method definitions.
- → Has inbuilt support for paging and sorting results.
- → Allows using JPQL for object-oriented queries and executing native queries.


## 020.SpringBoot-how-to-use-springdatajpa.md
🛠️ **How to use Spring Data JPA**
- → Add `spring-boot-starter-data-jpa` dependency to your Spring Boot application.
- → Add the appropriate database driver dependency (e.g., MySQL Connector, Oracle driver) to `pom.xml`.
- → Define a model class (entity) to represent a database table, annotated with JPA annotations like `@Entity`, `@Id`, etc.
- → Create a repository interface that extends one of the repository interfaces from Spring Data JPA (e.g., `JpaRepository`).
- → Specify the entity type and the primary key type in the repository interface (e.g., `JpaRepository<YourEntity, Long>`).
- → Use this repository interface in controllers or other layers to perform CRUD operations.
- → Define finder methods by simply declaring abstract methods in the repository interface (e.g., `findByFirstName(String firstName)`).
- → Spring Boot automatically creates a data source and all required beans based on dependencies and `application.properties`.
- → Spring Data JPA dynamically generates an implementation for the repository interface at runtime.

## 021.SpringBoot-create-coupon-microservice.md
🚀 **Creating Coupon Microservice Project**
- → Go to Spring Tool Suite: File -> New -> Spring Starter Project.
- → **Name:** `coupon-service`
- → **Group:** `com.bharath.springboot`
- → **Artifact:** `coupon-service`
- → **Description:** `Coupon Micro Service`
- → **Package:** `com.bharath.springboot`
- → **Dependencies:**
  - → `Spring Data JPA` (under SQL)
  - → `MySQL Driver`

📦 **Coupon Entity (`Coupon.java`)**
- → Create a new class `Coupon` in `src/main/java/com/bharath/springboot/model` (or `entities`).
- → Define fields:
  - → `private long id;`
  - → `private String code;`
  - → `private BigDecimal discount;`
  - → `private String exp_date;`
- → Generate getters and setters for all fields.
- → Annotate the class with `@Entity`.
- → Annotate the `id` field with `@Id`.
- → For auto-incrementing ID, use `@GeneratedValue(strategy = GenerationType.IDENTITY)`.
```java
package com.bharath.springboot.model;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import java.math.BigDecimal;

@Entity
public class Coupon {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private long id;
    private String code;
    private BigDecimal discount;
    private String exp_date;

    // Getters and Setters
    public long getId() {
        return id;
    }

    public void setId(long id) {
        this.id = id;
    }

    public String getCode() {
        return code;
    }

    public void setCode(String code) {
        this.code = code;
    }

    public BigDecimal getDiscount() {
        return discount;
    }

    public void setDiscount(BigDecimal discount) {
        this.discount = discount;
    }

    public String getExp_date() {
        return exp_date;
    }

    public void setExp_date(String exp_date) {
        this.exp_date = exp_date;
    }
}
```

🗄️ **Coupon Repository (`CouponRepo.java`)**
- → Create a new interface `CouponRepo` in `src/main/java/com/bharath/springboot/repos`.
- → Extend `JpaRepository`.
- → Specify the entity type (`Coupon`) and its primary key type (`Long`).
```java
package com.bharath.springboot.repos;

import com.bharath.springboot.model.Coupon;
import org.springframework.data.jpa.repository.JpaRepository;

public interface CouponRepo extends JpaRepository<Coupon, Long> {
    // Finder method example
    Coupon findByCode(String code);
}
```

⚙️ **Data Source Configuration (`application.properties`)**
- → Configure database connection details in `src/main/resources/application.properties`.
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/coupondb
spring.datasource.username=root
spring.datasource.password=testone,two,three,four
```

🧪 **Testing - Save Coupon**
- → Open the generated test class (e.g., `CouponServiceApplicationTests.java`).
- → Autowire `CouponRepo`.
- → Write a test method to save a new `Coupon` entity.
```java
package com.bharath.springboot;

import com.bharath.springboot.model.Coupon;
import com.bharath.springboot.repos.CouponRepo;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import java.math.BigDecimal;

import static org.junit.jupiter.api.Assertions.assertNotNull;

@SpringBootTest
class CouponServiceApplicationTests {

    @Autowired
    private CouponRepo repo;

    @Test
    void saveCouponTest() {
        Coupon coupon = new Coupon();
        coupon.setCode("SUPERSALE");
        coupon.setDiscount(new BigDecimal(20));
        coupon.setExp_date("12/12/2025");
        Coupon savedCoupon = repo.save(coupon);
        assertNotNull(savedCoupon.getId());
    }
}
```

🔍 **Testing - Finder Method (`findByCode`)**
- → Add `findByCode(String code)` abstract method to `CouponRepo` interface.
- → Write a test method to retrieve a coupon by its code.
```java
package com.bharath.springboot;

import com.bharath.springboot.model.Coupon;
import com.bharath.springboot.repos.CouponRepo;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import java.math.BigDecimal;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertNotNull;

@SpringBootTest
class CouponServiceApplicationTests {

    @Autowired
    private CouponRepo repo;

    // ... (saveCouponTest method from above)

    @Test
    void findByCodeTest() {
        Coupon coupon = repo.findByCode("SUPERSALE");
        assertNotNull(coupon);
        assertEquals(new BigDecimal(20), coupon.getDiscount());
    }
}
```

## 022.SpringBoot-what-is-orm.md
↔️ **Object-Relational Mapping (ORM)**
- → Process of mapping a Java class to a database table.
- → Maps class fields/members to database table columns.
- → Allows synchronization of class objects into database rows.
- → Developers interact with objects, not directly with SQL.
- → Methods like `save`, `update`, `delete` are invoked on objects.
- → ORM tools automatically generate SQL statements.
- → Uses JDBC internally for insertion, updation, deletion.
- → Eliminates direct SQL interaction and low-level JDBC API usage.
- → Simplifies data persistence by abstracting database operations.

## 023.SpringBoot-create-product-service-dal.md
🛒 **Create Product Service Data Access Layer**
- → Go to STS: File -> New -> Spring Starter Project.
- → **Name:** `product-service`
- → **Group:** `com.bharath.springboot`
- → **Artifact:** `product-service`
- → **Description:** `Product Micro Service`
- → **Package:** `com.bharath.springboot`
- → **Dependencies:**
  - → `MySQL Driver`
  - → `Spring Data JPA`

📦 **Product Entity (`Product.java`)**
- → Create a new class `Product` in `src/main/java/com/bharath/springboot/model`.
- → Define fields:
  - → `private long id;`
  - → `private String name;`
  - → `private String description;`
  - → `private BigDecimal price;`
- → Generate getters and setters.
- → Annotate class with `@Entity`.
- → Annotate `id` with `@Id` and `@GeneratedValue(strategy = GenerationType.IDENTITY)`.
```java
package com.bharath.springboot.model;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import java.math.BigDecimal;

@Entity
public class Product {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private long id;
    private String name;
    private String description;
    private BigDecimal price;

    // Getters and Setters
    public long getId() {
        return id;
    }

    public void setId(long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    public BigDecimal getPrice() {
        return price;
    }

    public void setPrice(BigDecimal price) {
        this.price = price;
    }
}
```

🗄️ **Product Repository (`ProductRepo.java`)**
- → Create a new interface `ProductRepo` in `src/main/java/com/bharath/springboot/repos`.
- → Extend `JpaRepository<Product, Long>`.
```java
package com.bharath.springboot.repos;

import com.bharath.springboot.model.Product;
import org.springframework.data.jpa.repository.JpaRepository;

public interface ProductRepo extends JpaRepository<Product, Long> {
}
```

⚙️ **Data Source Configuration (`application.properties`)**
- → Copy properties from `coupon-service`'s `application.properties`.
- → Change `couponDB` to `productDB`.
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/productdb
spring.datasource.username=root
spring.datasource.password=testone,two,three,four
```

🧪 **Testing**
- → (Self-exercise) Write a test in `src/test/java` to save a product to the database.
- → Inject `ProductRepo` and invoke its `save` method with product details.

## 024.SpringBoot-what-is-jpa.md
📚 **What is JPA**
- → JPA stands for Java Persistence API.
- → It is a standard from Oracle for performing object-relational mapping in Java EE applications.
- → JPA comes with a specification (for vendors) and an API (for developers).
- → Popular JPA providers (implementers of the API) include Hibernate, OpenJPA, EclipseLink.
- → Developers learn one single API, allowing switching between providers without application changes.
- → Hibernate is the most popular JPA provider.
- → JPA API includes classes like `EntityManagerFactory` and `EntityManager`.
- → Uses annotations like `@Entity`, `@Table`, `@Id`, `@Column` for mapping Java classes and fields to database tables and columns.
- → Spring Data JPA further abstracts `EntityManagerFactory` and `EntityManager`, simplifying development.

## 025.SpringBoot-what-are-entity-object-states.md
👻 **Entity Object States**
- → Entities (domain class objects) go through three states internally when managed by Hibernate:
  - → **Transient State:**
    - → Object is not yet associated with the underlying Hibernate session.
    - → Just created, not yet persisted.
  - → **Persistent State:**
    - → Object is associated with the Hibernate session.
    - → Achieved by invoking `save` on a new object or `find` on an existing record.
    - → Changes to the object are synchronized with the database.
  - → **Detached State:**
    - → Object was previously in a persistent state.
    - → No longer associated with the Hibernate session (e.g., after `close` or `clear` on `EntityManager`).
    - → Still exists in the database.
    - → Can be re-attached to a session by invoking `save` (or `merge`) with another session.
- → When an object is deleted, it moves from persistent to transient state (object exists, but record is gone from DB).
- → Objects in transient or detached states can be garbage collected if not re-associated.
- → Detached objects can be passed across application layers.

## 026.SpringBoot-what-are-jpa-associations.md
🔗 **JPA Associations**
- → Maps relationships between database tables to JPA entities.
- → Allows navigation from one entity to another, with SQL queries (joins) generated by JPA implementation.
- → **Four types of relationships:**
  - → **One-to-One:** A single instance of one entity relates to a single instance of another (e.g., Person and License).
  - → **Many-to-Many:** Multiple instances of one entity relate to multiple instances of another (e.g., Order and Product).
  - → **One-to-Many:** One instance of an entity relates to multiple instances of another (e.g., Customer to Phone Numbers).
  - → **Many-to-One:** Multiple instances of an entity relate to a single instance of another (e.g., Phone Number to Customer).
- → **Relationship Modes:**
  - → **Unidirectional:** Relationship can only be navigated in one direction (e.g., from Customer to Phone Number, but not vice-versa).
  - → **Bidirectional:** Relationship can be navigated in both directions (e.g., from Customer to Phone Number and Phone Number to Customer).
- → Configured using annotations: `@OneToOne`, `@OneToMany`, `@ManyToOne`, `@ManyToMany` with various properties.

## 027.SpringBoot-what-is-cascading.md
🌊 **What is Cascading**
- → Process of propagating non-select operations (insert, update, delete) from a main object in an association to its child objects.
- → Controlled by the `cascade` element on annotations like `@OneToMany`, `@ManyToOne`.
- → Takes different values:
  - → `CascadeType.PERSIST`: Propagates insert operations.
  - → `CascadeType.MERGE`: Propagates insert or update operations.
  - → `CascadeType.REMOVE`: Deletes child objects when the main object is deleted.
  - → `CascadeType.REFRESH`: Refreshes child objects when the main object is refreshed (using `EntityManager`).
  - → `CascadeType.DETACH`: Detaches child objects when the main object is detached (using `EntityManager`).
  - → `CascadeType.ALL`: Applies all cascade types (persist, merge, remove, refresh, detach).

## 028.SpringBoot-what-is-lazy-loading.md
⏳ **What is Lazy Loading**
- → When a parent object is loaded, associated child objects can be loaded either immediately (eager loading) or on demand (lazy loading).
- → **Eager Loading:** Child objects are fetched immediately along with the parent object.
- → **Lazy Loading:** Child objects are fetched only when they are first accessed in the application.
- → **Example:** If a `Person` object has a one-to-many association with `Phone Numbers` and phone numbers are configured for lazy loading:
  - → When the `Person` object is fetched, only the person's data is loaded.
  - → The list of phone numbers is not fetched from the database at this time.
  - → Only when `person.getPhoneNumbers()` is invoked, the phone numbers are then fetched from the database.
- → **Benefits:** Improves application performance by loading only necessary data, reducing initial load times and memory consumption.
- → **Configuration:** Controlled by the `fetch` attribute on JPA association annotations (e.g., `@OneToMany`, `@ManyToOne`).
  - → `FetchType.EAGER`: Loads associated data immediately.
  - → `FetchType.LAZY`: Loads associated data only when accessed for the first time.

## 029.SpringBoot-what-are-two-levels-of-caching.md
💾 **Two Levels of Caching in Hibernate**
- → Hibernate supports caching at two levels:
  - → **Level One Cache (L1 Cache):**
    - → Operates at the Hibernate `Session` level.
    - → Enabled by default and always present.
    - → Cache is specific to a single Hibernate `Session`.
    - → Data loaded from the database is stored in this cache for the current session.
    - → Subsequent requests for the same data within the *same* session will retrieve it from L1 cache, avoiding database hits.
    - → Not shared across different sessions or users.
  - → **Level Two Cache (L2 Cache):**
    - → Operates at the `SessionFactory` level.
    - → Requires additional configuration to enable.
    - → Cache is shared across multiple Hibernate `Session`s.
    - → Objects cached at this level are accessible by different user sessions.
    - → Significantly improves performance by reducing database access for frequently accessed data across the application.
    - → Needs a caching provider (e.g., Ehcache, Hazelcast) as Hibernate does not have built-in L2 cache implementation.
- → `SessionFactory` is responsible for creating multiple Hibernate `Session`s.
- → L2 cache acts as a common cache for all sessions created by that `SessionFactory`.
- → When L2 cache is configured, a session will first check L2 cache, then L1 cache, and finally the database.
- → Ehcache is a popular and powerful L2 caching provider.

## 030.SpringBoot-how-to-configure-second-level-cache.md
⚙️ **How to Configure Second Level Cache**
- → **Step 1: Choose a Caching Library:** Select a second-level caching library (e.g., Ehcache, Hazelcast).
- → **Step 2: Add Dependency:** Add the chosen caching library's dependency to `pom.xml`.
- → **Step 3: Create Configuration File:** Create a configuration file (e.g., `ehcache.xml` for Ehcache).
  - → Specify temporary storage location, maximum elements in memory, time-to-live for objects, etc.
- → **Step 4: Configure `application.properties`:**
  - → Set `spring.jpa.properties.hibernate.cache.use_second_level_cache=true`.
  - → Configure the Hibernate region factory class (e.g., `spring.jpa.properties.hibernate.cache.region.factory_class=org.hibernate.cache.ehcache.EhCacheRegionFactory`).
  - → Point to the configuration file (e.g., `spring.jpa.properties.hibernate.cache.provider_configuration_file_resource_path=ehcache.xml`).
- → **Step 5: Annotate Entities:** Mark entity classes to be cached with `@Cacheable` annotation.
  - → Optionally, specify a caching strategy (e.g., `@Cache(usage = CacheConcurrencyStrategy.READ_WRITE)`).

## 031.SpringBoot-what-is-aop.md
🎯 **What is AOP (Aspect-Oriented Programming)**
- → A programming paradigm that aims to increase modularity by allowing the separation of cross-cutting concerns.
- → **Cross-cutting concerns:** Non-functional requirements (e.g., security, logging, transaction management, profiling) that are typically spread across multiple layers and modules of an application.
- → In AOP, an **aspect** is the key unit, similar to how an object is the key unit in OOP.
- → An aspect is a specialized class that addresses a specific cross-cutting concern.
- → Aspects can be applied to classes and objects at runtime.
- → **Advantages of AOP:**
  - → **Addresses Cross-Cutting Concerns:** Centralizes common non-functional requirements.
  - → **Reusability:** Once developed, an aspect can be reused across different classes and applications.
  - → **Quick Development:** Developers can focus on business logic without worrying about non-functional requirements.
  - → **Specialization:** Allows developers to specialize in specific concerns (e.g., security aspect, logging aspect).
  - → **Runtime Control:** Aspects can be enabled or disabled at runtime through configuration.
- → Popular AOP frameworks in Java include Spring AOP and AspectJ.
- → Spring AOP has its own implementation and can also work with AspectJ.

## 032.SpringBoot-what-is-aop-terminology.md
📚 **AOP Terminology**
- → **Aspect:**
  - → A plain Java class marked with `@Aspect` (from AspectJ).
  - → Contains a number of advices and pointcuts.
  - → Addresses a particular cross-cutting concern (e.g., security, transaction management, logging).
- → **Advice:**
  - → A method within an aspect that addresses a part of the cross-cutting concern.
  - → Defines the action to be taken at a join point.
  - → **Spring AOP Advice Types (method level join points only):**
    - → `Before`: Executed before a method call.
    - → `After`: Executed after a method call (regardless of success or exception).
    - → `AfterReturning`: Executed after a method returns successfully.
    - → `AfterThrowing`: Executed after a method throws an exception.
    - → `Around`: Combines `Before` and `After`; executed both before and after a method call.
- → **Join Point:**
  - → A point in the Java program where an advice needs to be applied.
  - → In Spring AOP, join points are limited to method executions.
  - → Could conceptually be a method, field, or constructor (but Spring AOP only supports methods).
- → **Pointcut:**
  - → An expression language (like a regular expression) used to match a particular join point.
  - → Uses a syntax to express where advices should be applied (e.g., `execution(* *..*Service.*(..))`).
  - → Marked with `@Pointcut` annotation.

## 033.SpringBoot-how-to-implement-aop.md
🛠️ **How to Implement AOP**
- → **Step 1: Add AOP Dependency:** Include the Spring AOP starter dependency in your `pom.xml`.
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```
- → **Step 2: Create an Aspect:** Create a Java class and annotate it with `@Aspect` and `@Component` (or `@Configuration` if it contains other bean definitions).
```java
package com.bharath.springboot.aop;

import org.aspectj.lang.annotation.Aspect;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class LoggingAspect {
    // Advices and Pointcuts will go here
}
```
- → **Step 3: Define Pointcuts:** Use the `@Pointcut` annotation to define expressions that match join points.
```java
@Pointcut("execution(* com.bharath.springboot.service.*.*(..))")
public void serviceMethods() {}
```
- → **Step 4: Define Advices:** Implement methods within the aspect and annotate them with advice types (`@Before`, `@After`, `@AfterReturning`, `@AfterThrowing`, `@Around`).
  - → **Before Advice:** Executed before the matched method.
```java
import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.Before;

@Before("serviceMethods()")
public void logBefore(JoinPoint joinPoint) {
    System.out.println("Before executing: " + joinPoint.getSignature().getName());
}
```
  - → **After Returning Advice:** Executed after the matched method returns successfully.
```java
import org.aspectj.lang.annotation.AfterReturning;

@AfterReturning(pointcut = "serviceMethods()", returning = "result")
public void logAfterReturning(JoinPoint joinPoint, Object result) {
    System.out.println("After returning from: " + joinPoint.getSignature().getName() + ", Result: " + result);
}
```
  - → **After Throwing Advice:** Executed after the matched method throws an exception.
```java
import org.aspectj.lang.annotation.AfterThrowing;

@AfterThrowing(pointcut = "serviceMethods()", throwing = "exception")
public void logAfterThrowing(JoinPoint joinPoint, Throwable exception) {
    System.out.println("After throwing from: " + joinPoint.getSignature().getName() + ", Exception: " + exception.getMessage());
}
```
  - → **Around Advice:** Executed before and after the matched method, allowing full control over execution.
```java
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;

@Around("serviceMethods()")
public Object logAround(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
    System.out.println("Around before: " + proceedingJoinPoint.getSignature().getName());
    Object result = proceedingJoinPoint.proceed(); // Execute the actual method
    System.out.println("Around after: " + proceedingJoinPoint.getSignature().getName() + ", Result: " + result);
    return result;
}
```
- → **Step 5: Enable AspectJ Auto-Proxying (if not using Spring Boot auto-configuration):** For traditional Spring applications, you might need `@EnableAspectJAutoProxy` on a configuration class. Spring Boot typically enables this automatically.

## 034.SpringBoot-what-is-a-transaction.md
💸 **What is a Transaction**
- → A transaction is a sequence of operations that are performed as a single logical unit of work.
- → It adheres to an "all or nothing" principle: either all operations within the transaction are completed successfully, or none of them are.
- → **Example (Money Transfer):** If money is deducted from one account but the application crashes before it's credited to another, a transaction ensures the money is not lost.
- → Code is wrapped within a transaction boundary.
- → Changes are not made permanent until a `commit` operation.
- → If something goes wrong, the transaction can be `rolled back` to its previous state.
- → Supports `save points` to roll back to a specific intermediate state within a transaction.

## 035.SpringBoot-what-are-transaction-acid-properties.md
🔒 **Transaction ACID Properties**
- → **ACID** stands for:
  - → **Atomicity:** Everything happens or nothing happens. All operations within a transaction are treated as a single, indivisible unit. If any part fails, the entire transaction is rolled back.
  - → **Consistency:** Ensures that a transaction brings the database from one valid state to another. Data must always be in a consistent state before and after the transaction.
  - → **Isolation:** Concurrent transactions execute in isolation, meaning the intermediate state of one transaction is invisible to other transactions until it is committed. Prevents interference between concurrent operations.
  - → **Durability:** Once a transaction is committed, its changes are permanent and survive system failures (e.g., power outages, crashes). Committed data is written to non-volatile storage.

## 036.SpringBoot-what-are-distributed-transactions.md
✨ **Distributed Transactions**
- → A distributed transaction, also known as a Global transaction or an XA transaction, typically spans across different databases or even resources like messaging brokers.
- → **Example:** Money transfer between two different banks, where money deducted from one account must be safely transferred to another account or database.
- → This could also be within an organization, where applications use multiple databases and all work must be done within one single transaction.

⚙️ **Components**
- → **Transaction Manager:** Responsible for communication between the application and all resource managers.
- → **Resource Managers:** Know how to get work done by the underlying resources, like databases or messaging brokers.

🔄 **Two-Phase Commit (2PC)**
- → A mechanism used in distributed transactions where the entire transaction gets done in two phases.
- → **Phase 1 (Prepare Phase):**
    - → Each participating resource manager writes all records to a local temporary location.
    - → The transaction manager ensures all resource managers have written the data to a temporary location.
    - → Resource managers communicate any errors or an "OK" status to the transaction manager.
- → **Phase 2 (Commit/Rollback Phase):**
    - → The transaction manager checks if all resource managers are "OK".
    - → If all are "OK", it instructs each resource manager to commit everything they have written to the temporary location.
    - → If there is an issue, it asks all resource managers to roll back.

📝 **Summary**
- → Distributed transactions ensure that applications using multiple data sources can achieve an "all or nothing" outcome.
- → Everything should happen within the transaction boundary.
- → This is achieved using transaction managers and resource managers, employing the Two-Phase Commit mechanism.

## 037.SpringBoot-what-are-the-transaction-isolation-levels.md
✨ **Transaction Isolation Levels**
- → Control how multiple transactions running against a database isolate their underlying data.
- → Provide transactional concurrency, allowing multiple transactions to run in parallel without affecting each other.

🚨 **Database Anomalies**
- → **Dirty Reads:**
    - → Occurs when a transaction reads uncommitted data written by another transaction.
    - → If the first transaction rolls back, the data read by the second transaction becomes invalid.
- → **Non-Repeatable Reads:**
    - → Occurs when a transaction reads the same data twice and gets different values because another committed transaction modified the data between the two reads.
- → **Phantom Reads:**
    - → Occurs when a transaction reads a set of rows based on a `WHERE` clause, and then another transaction inserts new rows that satisfy the `WHERE` clause.
    - → When the first transaction re-executes the read, it finds new "phantom" rows.

📊 **Java EE Transaction Isolation Levels**
- → Four standard levels, each preventing different anomalies:
- → **TRANSACTION_READ_UNCOMMITTED:**
    - → Allows reading uncommitted data.
    - → All anomalies can occur: Dirty Reads, Non-Repeatable Reads, Phantom Reads.
- → **TRANSACTION_READ_COMMITTED:**
    - → Ensures transactions read only committed data.
    - → Prevents Dirty Reads.
    - → Non-Repeatable Reads and Phantom Reads can still occur.
- → **TRANSACTION_REPEATABLE_READ:**
    - → Prevents Dirty Reads and Non-Repeatable Reads.
    - → Phantom Reads can still occur.
    - → Often the default isolation level in many databases.
- → **TRANSACTION_SERIALIZABLE:**
    - → The most strict isolation level.
    - → The most strict isolation level.
    - → Prevents all anomalies: Dirty Reads, Non-Repeatable Reads, and Phantom Reads.
    - → Least performing due to high concurrency restrictions (e.g., potential table-level locks).
    - → Performance of the application decreases as the isolation level increases (from Read Uncommitted to Serializable).

📝 **Key Takeaways**
- → Database anomalies (Dirty Reads, Non-Repeatable Reads, Phantom Reads) can be avoided using different isolation levels.
- → `SERIALIZABLE` is the most strict and least performing isolation level.
- → `REPEATABLE_READ` is the most popular and frequently used transaction isolation level.

## 038.SpringBoot-what-is-optimistic-vs-pessimistic-locking.md
✨ **Optimistic vs Pessimistic Locking**
- → Both mechanisms determine how transactions in an application access underlying data in the database.

🛡️ **Optimistic Locking**
- → A specific record in the database table is open for all user sessions or transactions.
- → No database records are explicitly locked.
- → Relies on a `version number` or `timestamp` column added to each table.
- → When a transaction reads data, it also reads the version number/timestamp.
- → If another transaction updates the same record, it updates the version number/timestamp.
- → Before an update, the first transaction checks if the initially read version number/timestamp matches the current one in the database.
- → If they don't match, it means another transaction updated the data, and the current transaction must discard or redo its work by re-reading the data.
- → **Assumption:** It's optimistic that another transaction will not update the data.
- → **Use Case:** Very helpful when the application deals with a lot of reads and few updates.

🔒 **Pessimistic Locking**
- → The application explicitly locks records, a record, or even the entire table for read/write operations.
- → Only the current transaction can access the locked data.
- → Other transactions have to wait until the current transaction finishes its work because of the exclusive lock.
- → Provides better data integrity than optimistic locking.
- → **Consideration:** Applications must be carefully designed to avoid deadlock situations.
- → **Transaction Isolation Levels:** Appropriate isolation levels need to be set for pessimistic locking.
    - → `READ_COMMITTED` and `REPEATABLE_READ` are frequently used isolation levels with pessimistic locking.
    - → `READ_UNCOMMITTED` is generally not recommended due to dirty reads.
    - → `SERIALIZABLE` is too restrictive and performs poorly, though it prevents phantom reads.

📝 **Summary**
- → **Optimistic Locking:** Uses version numbers/timestamps; assumes low contention; no explicit locks; suitable for read-heavy, write-light scenarios.
- → **Pessimistic Locking:** Uses explicit database locks; assumes high contention; other transactions wait; provides strong integrity but requires careful deadlock handling.
