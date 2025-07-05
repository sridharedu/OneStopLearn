# Spring Cloud Combined Notes

## 001.Spring_Cloud-what-is-spring-cloud.md
✨ **What is Spring Cloud**
- → Spring Cloud is a collection of open-source components that address non-functional requirements for microservice applications built with Spring Boot.
- → **Non-functional requirements addressed:**
    - → **Service Registration and Discovery:** Microservices register themselves and can dynamically discover others at runtime.
    - → **Load Balancing:** Both client-side and provider/server-side load balancing.
    - → **Fault Tolerance:** Graceful handling of failures within a microservice.
    - → **Easy Integration:** Facilitates communication among microservices.
    - → **Cross-cutting Concerns:** Handles common concerns like security, logging, etc., in a single place.
    - → **Distributed Tracing:** Traces client requests as they flow through multiple microservices to identify issues.

## 002.Spring_Cloud-what-is-service-registration-and-discovery.md
✨ **What is Service Registration and Discovery**
- → When multiple microservice applications communicate, instead of remembering URLs and port numbers, they use a `Naming Server` component.
- → Microservices register themselves with the Naming Server by providing a unique application name/ID and their communication information (URL, port numbers).
- → The Naming Server acts as a registry of application IDs and their communication details.
- → Microservices that want to communicate with others can talk to the Naming Server using just the application ID to get the addresses and port numbers.
- → In Spring Cloud, the `Eureka Server` component acts as the Naming Server.
- → All microservice applications in Spring Cloud register themselves with Eureka Server.
- → Other microservices can dynamically discover and communicate through the Eureka Server.

## 003.Spring_Cloud-how-to-use-eureka-server.md
✨ **How to Use Eureka Server**
- → **Step 1: Create Eureka Server Project:**
    - → Create a new Spring Boot project.
    - → Add the `spring-cloud-starter-netflix-eureka-server` dependency.
    - → Annotate the main application class with `@EnableEurekaServer`.
    - → Configure the port on which the Eureka server will listen in `application.properties`.
- → **Step 2: Configure Microservice Clients:**
    - → In each microservice project, add the `spring-cloud-starter-netflix-eureka-client` dependency.
    - → In `application.properties` of the microservice, configure:
        - → `spring.application.name`: A unique name for the microservice to be recognized by Eureka.
        - → `eureka.client.serviceUrl.defaultZone`: The URL of the Eureka server for registration.
- → Once configured, microservices will register themselves with the Eureka server, and other microservices can discover them using their application names.

## 004.Spring_Cloud-how-to-do-client-side-load-balancing.md
✨ **How to Do Client-Side Load Balancing**
- → Client-side load balancing is where the distribution of load to a provider microservice is done directly on the client side.
- → In Spring Cloud, the `Load Balancer` component (formerly Ribbon) is used to accomplish this.
- → **Configuration:**
    - → Add the `spring-cloud-starter-loadbalancer` dependency to the project.
- → **Workflow:**
    - → The Load Balancer automatically looks at `application.properties` to find where the Eureka Server is running.
    - → When a RESTful call is made using a client like Feign Client (which integrates beautifully with the Load Balancer), the Load Balancer intercepts it.
    - → It fetches the details of the provider microservice's multiple running instances from the Eureka Server.
    - → It then automatically distributes the load across these instances, sending each request to a different service or provider instance.

## 005.Spring_Cloud-what-is-api-gateway.md
✨ **What is API Gateway**
- → The API Gateway component in Spring Cloud centralizes cross-cutting concerns for microservice applications.
- → Instead of repeating non-functional requirements like security, logging, tracing, and rate limiting across individual microservices, they are handled in one single place: the API Gateway.
- → All microservice applications must go through this API Gateway.
- → Custom filters (pre-filter, post-filter, error filter, route filter, etc.) can be written within the API Gateway to apply custom logic during routing.
- → This allows addressing cross-cutting concerns efficiently and consistently across all microservices.

## 006.Spring_Cloud-how-to-use-api-gateway.md
✨ **How to Use API Gateway**
- → **Step 1: Create API Gateway Project:**
    - → Create a new Spring Boot project.
    - → Add the `spring-cloud-starter-gateway` dependency.
- → **Step 2: Configure Microservice Routes:**
    - → Configure the microservice routes that should go through this API Gateway.
    - → The API Gateway acts as a client to the Eureka server to fetch the URL, port number, and other details for these microservice applications.
- → **Workflow:**
    - → All requests for these microservices will go through the API Gateway.
    - → Custom filter logic can be written within the API Gateway to address any cross-cutting concerns (e.g., security, logging, rate limiting) in a centralized manner.

## 007.Spring_Cloud-what-are-sleuth-and-zipkin.md
✨ **What are Sleuth and Zipkin**
- → **Sleuth:**
    - → Used for distributed tracing in Spring Cloud microservices.
    - → As microservice requests flow from one service to another, Sleuth automatically adds tracing information to logs.
    - → It adds an `application name`, `trace ID`, and other details to every log entry.
    - → This allows tracing the entire request flow from the client through all involved microservices.
    - → Helps in figuring out where exactly something went wrong and which application is responsible.
- → **Zipkin:**
    - → Compliments Sleuth by providing a beautiful UI/dashboard for distributed tracing.
    - → It visualizes the request flow and helps in analyzing where a request failed and why.
    - → Collects and aggregates trace data from Sleuth-instrumented applications, making it easy to inspect and troubleshoot distributed transactions.
