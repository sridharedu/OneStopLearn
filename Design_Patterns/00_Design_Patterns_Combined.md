# Design Patterns Combined Notes

## 001.Design_Patterns-what-are-the-design-patterns-you-have-used.md
✨ **Design Patterns Used**
- → **Data Access Object (DAO) Pattern:** Used to implement the data access layer, encapsulating data access logic in a simple class.
- → **Singleton Pattern:** Implemented for utility classes where only one instance of a particular class should be created.
- → **Prototype Pattern:** Used indirectly or directly, especially when using frameworks like Spring that allow configuring singleton or prototype beans.
- → **Dependency Injection (DI) and Inversion of Control (IoC) Patterns:** Utilized when working with Spring for dependency injection across layers.
- → **Model-View-Controller (MVC) Pattern:** Applied for the web layer, particularly with Spring Web.
- → **Factory Pattern:** Used with utility classes and factories to create instances of those classes.
- → Other patterns like Facade, Builder, Command can also be mentioned if used.

## 002.Design_Patterns-what-are-singleton-best-practices.md
✨ **Singleton Best Practices**
- → Make the constructor of the class private to prevent direct object creation.
- → Define a static field of the same class type globally.
- → Implement a static method to create an instance and assign it to the global static field, then return it.
- → Create the instance within a static block and acquire a class-level lock to ensure thread safety during instance creation.
- → Prevent cloning by implementing the `Cloneable` interface and overriding the `clone()` method to throw an exception (e.g., `CloneNotSupportedException`).
- → Prevent serialization and deserialization from creating new instances by implementing the `readResolve()` method to return the existing singleton instance.
