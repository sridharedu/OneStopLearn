## 026.Java-8-Features.md
✨ **Java 8 Features**
- → Lambda Expressions
- → Functional Interfaces
- → Default Methods
- → Predicates & Functions
- → Streaming API
## 027.Java-what-is-a-lambda.md
ƛ **Lambda Expressions**
- → Brings functional programming syntax to Java.
- → Anonymous functions without a name, return type, or access modifiers.
- → Simplifies code that would otherwise require many lines.
- → **Syntax:** `(parameters) -> { body }`
- → **Advantages:**
    - → Concise and reduces boilerplate code.
    - → Easy to implement anonymous inner classes on the fly.
    - → Can be passed as parameters to other methods.
## 028.Java-what-are-functional-interfaces.md
🧩 **Functional Interfaces**
- → An interface with exactly one abstract method.
- → Can have any number of `default` or `static` methods.
- → **Examples:** `Runnable` (with `run()`), `Comparator` (with `compare()`).
- → Can be marked with the `@FunctionalInterface` annotation, which causes a compile-time error if more than one abstract method is added.
- → A prerequisite for an interface to be represented as a lambda expression.
## 029.Java-what-is-the-use-of-lambda.md
💡 **Lambda Use Case: Simplifying Threads**
- → **Traditional Way:**
    - → Create a separate class that implements `Runnable`.
    - → Override the `run()` method.
    - → Instantiate the class.
    - → Pass the instance to a new `Thread` object.
    - → Call `thread.start()`.
- → **With Lambdas:**
    - → The `Runnable` interface is functional, so it can be expressed as a lambda.
    - → The entire class creation is replaced by a single lambda expression.
```java
// Old way requires a whole separate class definition.

// Lambda way
Runnable r = () -> {
    System.out.println("Thread logic here.");
};
new Thread(r).start();
```
## 030.Java-what-is-a-predicate.md
🤔 **Predicate**
- → A function that takes a single argument and returns a `boolean` value.
- → Represented by the `java.util.function.Predicate<T>` functional interface.
- → Its single abstract method is `boolean test(T t)`.
- → Since it's a functional interface, it can be created using a lambda expression.
```java
// Predicate to check if a number is greater than 20
Predicate<Integer> p = i -> i > 20;
boolean result = p.test(25); // returns true

// Predicate to check string length
Predicate<String> s = str -> str.length() > 5;
boolean s_result = s.test("Hello"); // returns false
```
## 031.Java-what-are-predicate-joins.md
🔗 **Predicate Joins**
- → Combine multiple predicates into a single, more complex predicate.
- → **`and(other)`**: Returns a composed predicate that represents a short-circuiting logical AND of this predicate and another.
- → **`or(other)`**: Returns a composed predicate that represents a short-circuiting logical OR of this predicate and another.
- → **`negate()`**: Returns a predicate that represents the logical negation of this predicate.
```java
Predicate<Integer> greaterThan10 = i -> i > 10;
Predicate<Integer> isEven = i -> i % 2 == 0;

// Numbers > 10 AND even
greaterThan10.and(isEven).test(12); // true

// Numbers > 10 OR even
greaterThan10.or(isEven).test(8); // true

// Numbers NOT > 10 (i.e., <= 10)
greaterThan10.negate().test(5); // true
```
## 032.Java-what-is-a-function.md
⚙️ **Function**
- → Represents a function that accepts one argument and produces a result.
- → Similar to a `Predicate`, but its return type is generic, not fixed to `boolean`.
- → Represented by the `java.util.function.Function<T, R>` functional interface, where `T` is the input type and `R` is the return type.
- → Its single abstract method is `R apply(T t)`.
```java
// Function to get the length of a string
Function<String, Integer> f = s -> s.length();
Integer len = f.apply("Hello World"); // returns 11
```
## 033.Java-what-are-default-methods-on-interfaces.md
🛡️ **Default Methods in Interfaces**
- → A method in an interface that has an implementation.
- → Introduced in Java 8 to allow adding new methods to existing interfaces without breaking implementing classes.
- → **Problem Solved:** Before Java 8, adding a new abstract method to an interface would force all implementing classes to provide an implementation, otherwise, they would fail to compile.
- → **Solution:** With `default` methods, you can add new functionality with a default implementation.
- → Implementing classes can choose to override the default method or just use the default behavior.
## 034.Java-class-implementing-two-interfaces-with-same-default-method.md
⚔️ **Interfaces with Same Default Method**
- → A class **can** implement two interfaces that have a default method with the same signature, but with a condition.
- → **The Diamond Problem:** If a class simply implements both interfaces, it results in a compilation error because the compiler doesn't know which default implementation to inherit.
- → **The Solution:** The implementing class **must override** the conflicting default method and provide its own implementation. This resolves the ambiguity.
```java
interface A {
    default void m1() { System.out.println("A"); }
}
interface X {
    default void m1() { System.out.println("X"); }
}

class B implements A, X {
    // This override is mandatory to resolve the conflict
    @Override
    public void m1() {
        System.out.println("B's implementation");
        // Optionally, you can even call a specific one:
        // A.super.m1();
    }
}
```
## 035.Java-how-to-use-stream-filter.md
💧 **Stream `filter()`**
- → A core intermediate operation in the Stream API.
- → Used to select elements from a stream that match a given `Predicate`.
- → **Workflow:**
    - → 1. Get a stream from a collection (`list.stream()`).
    - → 2. **Configure:** Use `filter()` with a predicate lambda (`.filter(n -> n % 2 == 0)`).
    - → 3. **Process:** Collect the results into a new collection (`.collect(Collectors.toList())`).
```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6);
List<Integer> evenNumbers = numbers.stream()
                                     .filter(n -> n % 2 == 0)
                                     .collect(Collectors.toList());
// evenNumbers will contain [2, 4, 6]
```
## 036.Java-other-methods-on-stream.md
🌊 **Other Stream Methods**
- → **`count()`**: A terminal operation that returns the number of elements in the stream. Often used after a `filter()`.
- → **`sorted()`**: An intermediate operation that returns a stream consisting of the elements of this stream, sorted according to natural order or a provided `Comparator`.
- → **`max(Comparator)`**: A terminal operation that returns the maximum element of the stream according to the provided `Comparator`.
- → **`min(Comparator)`**: A terminal operation that returns the minimum element of the stream according to the provided `Comparator`.
```java
List<String> names = List.of("John", "Jane", "Adam", "Eve");

// Count names longer than 3 chars
long count = names.stream().filter(s -> s.length() > 3).count(); // 2

// Sort names alphabetically
List<String> sorted = names.stream().sorted().collect(Collectors.toList());

// Find the longest name
Optional<String> max = names.stream().max(Comparator.comparing(String::length));
```
## 037.Java-map-vs-filter.md
🗺️ **`map()` vs. `filter()`**
- → Both are intermediate stream operations, but they serve different purposes.
- → **`filter(Predicate<T>)`**:
    - → **Purpose:** To select elements.
    - → **Action:** It tests each element against a predicate and keeps only those that return `true`.
    - → **Output:** A stream of the *same type* (`Stream<T>`), but potentially with fewer elements.
- → **`map(Function<T, R>)`**:
    - → **Purpose:** To transform elements.
    - → **Action:** It applies a function to each element, transforming it into a new element.
    - → **Output:** A stream of a potentially *different type* (`Stream<R>`) with the same number of elements as the input stream.
```java
List<String> names = List.of("John", "Jane", "Adam");

// filter: get names with 4 letters
List<String> filtered = names.stream()
                             .filter(s -> s.length() == 4)
                             .collect(Collectors.toList()); // ["John", "Jane"]

// map: get the length of each name
List<Integer> mapped = names.stream()
                           .map(s -> s.length())
                           .collect(Collectors.toList()); // [4, 4, 4]
```
## 038.Java-private-methods-in-interfaces.md
🤫 **Private Methods in Interfaces**
- → Since Java 9, interfaces can include `private` and `private static` methods.

- → **Primary Purpose: Code Reusability & Encapsulation**
  - → To share common code between `default` methods within an interface, preventing code duplication.
  - → To hide implementation details from the classes that implement the interface. The private methods are not part of the public API.

- → **Types of Private Methods:**
  - → **`private` instance methods:** Can only be called by `default` (non-static) methods within the interface.
  - → **`private static` methods:** Can be called by both `default` and `static` methods within the interface.

- → **Example:**
```java
interface AdvancedInterface {

    // Public static method
    static void staticMethod() {
        System.out.println("Public static method calling private static method:");
        privateStaticCommonLogic();
    }

    // Public default method
    default void defaultMethod1() {
        System.out.println("Default method 1 calling private instance method:");
        privateInstanceCommonLogic();
        System.out.println("Default method 1 calling private static method:");
        privateStaticCommonLogic();
    }

    // Another public default method
    default void defaultMethod2() {
        System.out.println("Default method 2 calling private instance method:");
        privateInstanceCommonLogic();
    }

    // Private instance method (can only be called by default methods)
    private void privateInstanceCommonLogic() {
        System.out.println("Executing private instance logic.");
    }

    // Private static method (can be called by default and static methods)
    private static void privateStaticCommonLogic() {
        System.out.println("Executing private static logic.");
    }
}
```
## 039.Java-immutable-collections.md
🛡️ **Immutable Collections**
- → **Before Java 9:** Creating immutable collections was verbose, requiring the use of the `Collections` utility class (e.g., `Collections.unmodifiableList(myList)`).
- → **Starting with Java 9:** Convenient factory methods `of()` were added to collection interfaces like `List`, `Set`, and `Map`.
- → These methods create compact, unmodifiable collections in a single line.
- → Attempting to modify a collection created with `of()` will result in an `UnsupportedOperationException`.
```java
// Creates an immutable list
List<String> immutableList = List.of("one", "two", "three");

// Creates an immutable set
Set<Integer> immutableSet = Set.of(1, 2, 3);
```
## 040.Java-stream-api-updates.md
🌊 **Stream API Updates (Java 9)**
- → **`takeWhile(Predicate)`**: Returns a stream consisting of the initial elements that match the predicate. It stops taking elements as soon as the predicate returns `false` for the first time.
- → **`dropWhile(Predicate)`**: The opposite of `takeWhile`. It discards the initial elements that match the predicate and returns a stream of the remaining elements, starting with the first one for which the predicate is `false`.
- → **`ofNullable(T t)`**: Returns a stream containing a single element if the element is non-null, otherwise returns an empty stream. Useful for avoiding `NullPointerException` when creating streams from potentially null values, especially within a `flatMap` operation.
