# REST API Combined Notes

## 001.REST_API-what-is-rest.md
✨ **What is REST**
- → REST stands for Representational State Transfer.
- → It is a set of principles or architectural guidelines.
- → HTTP is commonly used to implement these guidelines.
- → **First Principle: Uniform Interface and Easy Access**
    - → CRUD operations (Create, Read, Update, Delete) are performed using HTTP methods (verbs).
    - → **HTTP Methods (Verbs):**
        - → `POST`: Used to create a resource (e.g., `POST /employees`). Returns `201 Created`.
        - → `GET`: Used to read a resource (e.g., `GET /employees/{id}`). Returns `200 OK`.
        - → `PUT`: Used to update a complete object (e.g., `PUT /employees/{id}`).
        - → `PATCH`: Used for partial updates of an object (e.g., `PATCH /employees/{id}`).
        - → `DELETE`: Used to delete a resource (e.g., `DELETE /employees/{id}`).
    - → **URIs (Nouns):** HTTP provides URIs to easily identify any resource.
- → **Second Principle: Support Multiple Formats**
    - → RESTful Web services or APIs should support multiple data formats (e.g., XML, JSON, text).
- → **Summary:**
    - → REST is a combination of principles/architectural guidelines, mainly having a single interface.
    - → HTTP is a great way to implement REST due to its methods (verbs) and URIs (easy access).
    - → It should support multiple representations (different data types).

## 002.REST_API-http-put-vs-post-and-patch.md
✨ **HTTP PUT vs POST and PATCH**
- → **HTTP POST:**
    - → Primarily used for creating a resource.
    - → Although `PUT` can also create, `POST` is the common convention for creation.
- → **HTTP PUT:**
    - → Primarily used for updating a complete object.
- → **HTTP PATCH:**
    - → Used for partial updates.
    - → If only one or two fields on an object need to be updated, `PATCH` is used.

## 003.REST_API-how-to-create-rest-api.md
✨ **How to Create REST API with Spring Boot**
- → **Step 1: Create a Spring Boot Project**
    - → Add `spring-boot-starter-web` dependency (contains RESTful API annotations).
    - → Add appropriate data access layer dependencies (e.g., `spring-data-jpa`, `spring-jdbc`, and driver JARs).
- → **Step 2: Create Data Access Layer**
    - → Implement data access using Spring Data JPA or other chosen technology.
- → **Step 3: Create RESTful Controller**
    - → Create a controller class.
    - → Annotate the class with `@RestController`.
    - → Map the controller to a URI using `@RequestMapping` at the class level.
    - → **Example:**
```java
@RestController
@RequestMapping("/api/employees")
public class EmployeeController {
    // ... methods
}
```
- → **Step 4: Bind Java Methods to HTTP Methods**
    - → Use specific annotations to bind Java methods to HTTP methods and URIs.
    - → `@PostMapping`: For HTTP POST requests (e.g., creating a resource).
    - → `@GetMapping`: For HTTP GET requests (e.g., reading a resource).
    - → `@PutMapping`: For HTTP PUT requests (e.g., updating a resource).
    - → `@DeleteMapping`: For HTTP DELETE requests (e.g., deleting a resource).
    - → `@PatchMapping`: For HTTP PATCH requests (e.g., partial updates).
    - → Provide a URI to which the method should be bound.
    - → **Example (Creating an Employee):**
```java
@PostMapping
public ResponseEntity<Employee> createEmployee(@RequestBody Employee employee) {
    // ... save employee
    return new ResponseEntity<>(employee, HttpStatus.CREATED);
}
```
    - → **Example (Reading an Employee by ID with Path Variable):**
```java
@GetMapping("/{id}")
public ResponseEntity<Employee> getEmployeeById(@PathVariable("id") Long id) {
    // ... find employee by id
    return new ResponseEntity<>(employee, HttpStatus.OK);
}
```
    - → `@PathVariable`: Used to extract values from the URI path and inject them into method parameters.
- → **Summary:** Create application with right dependencies, mark controller with `@RestController` and `@RequestMapping`, then create API methods using appropriate HTTP method annotations and URI mappings.
