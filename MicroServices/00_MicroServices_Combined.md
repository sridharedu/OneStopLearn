# MicroServices Combined Notes

## 001.Microservices-what-is-a-monolithic-application.md
✨ **What is a Monolithic Application**
- → A monolithic application is where different modules of a software are combined into a single application or codebase.
- → **Example:** A hospital management software with patient registration, clinical, bed management, and claim management modules all in one codebase.
- → Over time, as the codebase grows, it becomes very difficult to fix defects or add new features.
- → Fixing a bug in one module requires ensuring no other modules are impacted.
- → To add a new feature or fix a bug, the entire application must be brought down for build and deployment, affecting all modules.

## 002.Microservices-what-are-microservices.md
✨ **What are Microservices**
- → Microservices are small and focused applications that bring together pieces that change for the same reason.
- → **Example:** A monolithic hospital management software can be split into four microservices: patient registration, clinical, bed management, and claim management.
- → Code boundaries are defined by business problems these microservices solve.
- → They have tiny codebases for the specific problem they are solving.
- → They are autonomous, meaning they can be built and deployed independently without impacting other microservices.
- → If they need to communicate, they do so using network calls through APIs exposed by other microservices.
- → An application is a microservice if it can be changed and deployed without changing or impacting another service.

## 003.Microservices-why-microservices.md
✨ **Why Microservices**
- → **Heterogeneity:** Different microservice applications can be built in different programming languages and deployed to different operating systems, unlike monolithic applications.
- → **Communication:** Microservices can communicate with each other using RESTful APIs, messaging, etc.
- → **Robustness:** Even if one microservice goes down, other microservices continue to deliver their functionality.
- → **Statelessness:** Microservice applications typically do not maintain state, enabling easy scalability.
- → **Scalability:** When the load increases, different instances of the same microservice can be deployed to handle the load.
- → **Easy Deployment:** Changes or new features can be easily deployed to production because only the specific microservice needs to be dealt with, increasing time to production.
- → **Reusable and Replaceable:** The same microservice can be reused wherever required, and due to loose coupling, one microservice can be easily replaced with another if needed.

## 004.Microservices-rest-vs-messaging.md
✨ **REST vs Messaging**
- → **REST (Representational State Transfer):**
    - → Usually good for synchronous request-response scenarios.
    - → Uses HTTP protocol, which is easy to implement for request-response.
    - → Ideal for developing external-facing APIs exposed outside the organization.
- → **Messaging:**
    - → Can be used for asynchronous communication.
    - → Suitable for non-blocking type applications.
    - → Much more reliable because messages can be persisted or stored and will not be lost.
    - → Usually used for applications within the organization or for microservices that communicate internally.
