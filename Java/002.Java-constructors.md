🧱 **Constructors**
- → A special method to initialize an object's properties upon creation.
- → Must have the same name as the class it is in.
- → Invoked only once when an object instance is created.

↔️ **Constructors vs. Methods**
- → **Name:** Constructor name must match the class name; method names can be different.
- → **Invocation:** Constructors are called automatically at object creation; methods must be called explicitly.
- → **Frequency:** A constructor runs once per object; methods can be called any number of times.

🔄 **Constructor Chaining (Same Class)**
- → Use `this()` to call another constructor within the same class.
- → `this()` must be the very first statement inside the calling constructor.
```java
class Accord {
    Accord() {
        this("V6"); // Calls the parameterized constructor
        System.out.println("Default constructor called.");
    }

    Accord(String engine) {
        System.out.println("Engine type: " + engine);
    }
}
```

🔼 **Invoking Superclass Constructor**
- → Use `super()` to call the constructor of the parent (superclass).
- → `super()` must be the very first statement in the child class's constructor.
```java
class Honda {
    Honda() {
        System.out.println("Honda constructor called.");
    }
}

class Accord extends Honda {
    Accord() {
        super(); // Calls the Honda() constructor
        System.out.println("Accord constructor called.");
    }
}
```
