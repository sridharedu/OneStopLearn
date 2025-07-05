## 041.Java-enhancements-to-try-with-resource.md
📦 **`try-with-resources` Enhancements**
- → **Before Java 9:** The resource to be managed had to be declared and initialized *inside* the `try` parentheses.
```java
// Pre-Java 9
MyResource resource1 = new MyResource();
try (MyResource r = resource1) {
    // ... use r ...
}
```
- → **Starting with Java 9:** The `try-with-resources` statement can manage a resource that is a final or effectively final variable, without needing to declare it inside the `try` block.
```java
// Java 9 and later
MyResource resource1 = new MyResource();
try (resource1) { // Much more concise
    // ... use resource1 ...
}
```
## 042.Java-10-release-cycle.md
⏳ **Java Release Cadence (Since Java 10)**
- → Starting with Java 10 (March 2018), Oracle moved to a **six-month rapid release cycle**.
- → **Before Java 10:** Releases were feature-driven and could take several years (e.g., Java 8 to 9 took over 3 years).
- → **After Java 10:** New versions are released every March and September.
- → **Benefit:** This allows new features to be delivered to developers much faster, in smaller, more manageable chunks.
- → **Java 10 Key Features:**
  - → Local-Variable Type Inference (`var`)
  - → Unmodifiable Collections via `Collectors` API
## 043.Java-var-type-inference.md
🔍 **`var` - Local-Variable Type Inference**
- → Introduced in Java 10, `var` allows the compiler to **infer** the type of a local variable from the right-hand side of its declaration.
- → **It is NOT like JavaScript's `var`:**
  - → The type is determined at compile time and is **immutable**. You cannot assign a different type to the variable later.
  - → It is **not a keyword**. This was a deliberate choice to avoid breaking older code that might have used "var" as a variable name.

- → **Benefits:**
  - → Reduces boilerplate and improves readability, especially with complex generic types.
  - → `var list = new ArrayList<Map<String, Integer>>();` is much cleaner than repeating the type on the left.

- → **Limitations:**
  - → Can only be used for **local variables** (inside methods, loops, etc.).
  - → Cannot be used for class fields, method parameters, or return types.
  - → Cannot be used to declare a variable without an initializer (`var x;` is illegal).
  - → Cannot be used for lambda expressions directly (`var lambda = () -> ...` is illegal because the target type must be explicit).
## 044.Java-collectors-api-updates.md
🛡️ **Collectors API Updates (Java 10)**
- → Java 10 introduced new methods in the `java.util.stream.Collectors` class to easily create **unmodifiable** collections from a stream.
- → **New Methods:**
  - → `Collectors.toUnmodifiableList()`
  - → `Collectors.toUnmodifiableSet()`
  - → `Collectors.toUnmodifiableMap()`
- → **Benefit:** Provides a direct and concise way to gather stream elements into a truly immutable collection.
- → Any attempt to modify the resulting collection (e.g., using `add()` or `remove()`) will throw an `UnsupportedOperationException`.
```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);

// Create an unmodifiable list of even numbers
List<Integer> evenNumbers = numbers.stream()
                                     .filter(n -> n % 2 == 0)
                                     .collect(Collectors.toUnmodifiableList());

// This line would throw UnsupportedOperationException
// evenNumbers.add(8);
```
## 045.Java-11-introduction.md
✨ **Java 11 Introduction**
- → A Long-Term Support (LTS) release, making it a popular choice for production applications.
- → **Key Updates:**
  - → New convenience methods added to the `String` API.
  - → Simplified file I/O with new methods on the `Files` class.
  - → Addition of `isEmpty()` method to the `Optional` class.
  - → Deprecations and removals of older modules (e.g., Java EE and CORBA modules).
## 046.Java-string-api-updates.md
📜 **String API Updates (Java 11)**
- → **`isBlank()`**: Returns `true` if the string is empty or contains only white space characters. More comprehensive than `isEmpty()`.
- → **`lines()`**: Returns a stream of strings, which are the lines extracted from the original string, separated by line terminators.
- → **`strip()`, `stripLeading()`, `stripTrailing()`**: Removes whitespace from the beginning and/or end of a string. Unlike `trim()`, these methods are Unicode-aware and can remove various kinds of whitespace.
- → **`repeat(int)`**: Returns a string whose value is the concatenation of this string repeated the given number of times.
```java
"\u2000  hello  ".strip(); // returns "hello"
"-=".repeat(3); // returns "-=-=-="
```
## 047.Java-file-api-updates.md
📁 **File API Updates (Java 11)**
- → Java 11 simplifies reading from and writing to files with new static methods on the `java.nio.file.Files` class.
- → **`writeString(Path, CharSequence)`**: Writes a sequence of characters to a file. It's a concise way to write a string to a file.
- → **`readString(Path)`**: Reads all content from a file into a string.
- → These methods make basic file operations much more straightforward.
```java
Path filePath = Path.of("myFile.txt");

// Write a string to a file
Files.writeString(filePath, "Hello, Java 11!");

// Read the entire file back into a string
String content = Files.readString(filePath);
```
## 048.Java-optional-isEmpty-method.md
🤔 **`Optional.isEmpty()`**
- → Java 11 added the `isEmpty()` method to the `java.util.Optional` class.
- → **Before Java 11:** To check if an `Optional` was empty, you had to use the negation of `isPresent()` (e.g., `!optional.isPresent()`).
- → **With Java 11:** The `isEmpty()` method provides a more direct and readable way to check for the absence of a value.
- → It returns `true` if the `Optional` is empty, and `false` otherwise.
```java
Optional<String> emptyOptional = Optional.empty();

// Old way
if (!emptyOptional.isPresent()) { ... }

// New, more readable way
if (emptyOptional.isEmpty()) { ... }
```
## 049.Java-string-api-updates.md
📜 **String API Updates (Java 12)**
- → **`indent(int n)`**:
  - → Adds `n` leading whitespace characters to the string.
  - → If `n` is negative, it attempts to remove up to `n` leading whitespace characters.
- → **`transform(Function<? super String, ? extends R> f)`**:
  - → Applies a provided function to the string.
  - → This allows for a chain of arbitrary operations to be applied to a string.
  - → The function can return a result of any type, not just a `String`.
```java
"Hello".indent(4); // returns "    Hello"

String json = "{\"name\":\"John\", \"age\":30}";
// Example: transform a JSON string into a User object
User user = json.transform(jsonString -> new Gson().fromJson(jsonString, User.class));
```
## 050.Java-compact-number-format.md
🔢 **Compact Number Formatting**
- → Java 12 introduced a new, concise way to format numbers based on a given `Locale`.
- → Use `NumberFormat.getCompactNumberInstance(Locale, NumberFormat.Style)` to get a formatter.
- → **Style:**
  - → `SHORT`: Abbreviates the number (e.g., "1K", "1M").
  - → `LONG`: Spells out the number (e.g., "1 thousand", "1 million").
```java
NumberFormat shortFormat = NumberFormat.getCompactNumberInstance(Locale.US, NumberFormat.Style.SHORT);
NumberFormat longFormat = NumberFormat.getCompactNumberInstance(Locale.US, NumberFormat.Style.LONG);

shortFormat.format(1000);  // returns "1K"
longFormat.format(1000);   // returns "1 thousand"

shortFormat.format(1_000_000); // returns "1M"
longFormat.format(1_000_000);  // returns "1 million"
```

## 051.Java-more-unicode-chars.md
🀄 **Unicode 11 Support**
- → Java 12 added support for Unicode 11.
- → This update includes thousands of new characters, symbols, and emoji.
- → Notable additions include:
  - → New scripts and characters for various languages, including more CJK Unified Ideographs.
  - → Symbols for chess and other board games.
  - → Additional emoji characters.
## 052.Java-collectors-api-updates.md
🎛️ **Collectors.teeing()**
- → A new static method in the `Collectors` class that allows collecting from a stream using two downstream collectors, then merging their results using a `BiFunction`.
- → **Use Case:** When you need to perform two different collecting operations on the same stream in a single pass.
- → **Parameters:**
  - → `downstream1`: The first collector.
  - → `downstream2`: The second collector.
  - → `merger`: A `BiFunction` to merge the results of the two collectors.
```java
// Example: Calculate the sum and count of numbers in a single pass
record Result(long sum, long count) {}

Result result = Stream.of(1, 2, 3, 4, 5)
    .collect(Collectors.teeing(
        Collectors.summingLong(i -> i), // Downstream 1: sum
        Collectors.counting(),          // Downstream 2: count
        Result::new                     // Merger: combine sum and count
    ));
// result will be Result[sum=15, count=5]
```
## 053.Java-sealed-classes.md
🛡️ **Sealed Classes and Interfaces (Preview Feature)**
- → A preview feature in Java 15 that allows you to restrict which other classes or interfaces may extend or implement them.
- → **Purpose:** Provides more fine-grained control over inheritance, enabling a class author to declare which classes are permitted to be direct subclasses.
- → **Keywords:**
  - → `sealed`: Applied to a class or interface declaration to indicate it is sealed.
  - → `permits`: Used after the `sealed` class/interface declaration to specify the exact list of classes that are allowed to extend or implement it.
- → **Rules for Permitted Subclasses:**
  - → They must be in the same module or package as the sealed class.
  - → They must explicitly extend the sealed class.
  - → They must be marked as either `final`, `sealed`, or `non-sealed`.
```java
// The interface is sealed and only permits specific implementations
public sealed interface Shape
    permits Circle, Square {
}

// Permitted subclasses must be final, sealed, or non-sealed
public final class Circle implements Shape { ... }

public non-sealed class Square implements Shape { ... }
```
## 054.Java-record-enhancements.md
🧱 **Record Enhancements (Java 15)**
- → The `record` type, still a preview feature in Java 15, received several enhancements.
- → **Sealed Records:** Records can now implement `sealed` interfaces. This allows you to use the conciseness of records within a restricted inheritance hierarchy.
- → **Local Records:** You can now declare records inside methods or inner classes.
- → **Annotations on Record Components:** It is now possible to apply annotations to the components of a record, which is useful for validation, serialization, etc.
```java
// A sealed interface
sealed interface Shape permits Circle, Rectangle {}

// An annotation for validation
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.RECORD_COMPONENT)
@interface Positive {}

// A record implementing the sealed interface and using an annotation
public record Circle(@Positive double radius) implements Shape {}

// A record can also be declared locally inside a method
public void someMethod() {
    record Point(int x, int y) {}
    Point p = new Point(10, 20);
}
```