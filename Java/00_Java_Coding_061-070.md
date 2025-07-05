## 061.Java-coding-problem-6.md
## 📝 Coding Problem 6: Find the Largest Element in an Array

**Problem Statement:**
Given an array of integers, find the largest element in the array.

**Example:**
Input: `int[] arr = {10, 5, 20, 8, 15};`
Output: `20`

---

### Approach 1: Pre-Java 8 (Imperative Style - Iteration)

**Thought Process:**
The most straightforward way to find the largest element is to iterate through the array, keeping track of the maximum value encountered so far. Initialize a variable with the smallest possible integer value (or the first element of the array) and update it if a larger element is found.

**Implementation:**
```java
public class LargestElementPreJava8 {

    public static int findLargestElement(int[] arr) {
        if (arr == null || arr.length == 0) {
            throw new IllegalArgumentException("Array cannot be null or empty.");
        }

        int max = arr[0]; // Assume the first element is the largest
        for (int i = 1; i < arr.length; i++) {
            if (arr[i] > max) {
                max = arr[i];
            }
        }
        return max;
    }

    public static void main(String[] args) {
        int[] arr1 = {10, 5, 20, 8, 15};
        System.out.println("Largest element in arr1: " + findLargestElement(arr1)); // 20

        int[] arr2 = {-1, -5, -2};
        System.out.println("Largest element in arr2: " + findLargestElement(arr2)); // -1
    }
}
```

---

### Approach 2: Post-Java 8 (Functional Style - Using Streams)

**Thought Process:**
Java 8 Streams provide a concise way to find the maximum element using the `max()` terminal operation. This method takes a `Comparator` to define the comparison logic. For primitive types like `int`, `IntStream` offers a direct `max()` method.

**Implementation:**
```java
import java.util.Arrays;
import java.util.OptionalInt;

public class LargestElementPostJava8 {

    public static int findLargestElement(int[] arr) {
        if (arr == null || arr.length == 0) {
            throw new IllegalArgumentException("Array cannot be null or empty.");
        }

        OptionalInt max = Arrays.stream(arr)
                                .max();

        // OptionalInt.getAsInt() should only be called if isPresent() is true
        // Since we handle empty array above, it will always be present here.
        return max.getAsInt();
    }

    public static void main(String[] args) {
        int[] arr1 = {10, 5, 20, 8, 15};
        System.out.println("Largest element in arr1: " + findLargestElement(arr1)); // 20

        int[] arr2 = {-1, -5, -2};
        System.out.println("Largest element in arr2: " + findLargestElement(arr2)); // -1
    }
}
```

---

### Discussion on Approaches:

- → **Readability:** Both approaches are quite readable. The imperative approach is explicit about the loop and comparison. The Stream API approach is more declarative and concise, especially for those familiar with streams.
- → **Conciseness:** The Stream API solution is more compact.
- → **Performance:** For primitive arrays, the imperative loop is often slightly more performant due to less overhead compared to stream creation and processing. However, for larger datasets or more complex operations, the Stream API (especially parallel streams) can offer benefits.
- → **Error Handling:** Both approaches handle null or empty arrays by throwing an `IllegalArgumentException`.
## 062.Java-coding-problem-7.md
## 📝 Coding Problem 7: Find the Smallest Element in an Array

**Problem Statement:**
Given an array of integers, find the smallest element in the array.

**Example:**
Input: `int[] arr = {10, 5, 20, 8, 15};`
Output: `5`

---

### Approach 1: Pre-Java 8 (Imperative Style - Iteration)

**Thought Process:**
Similar to finding the largest element, we can iterate through the array, keeping track of the minimum value encountered so far. Initialize a variable with the largest possible integer value (or the first element of the array) and update it if a smaller element is found.

**Implementation:**
```java
public class SmallestElementPreJava8 {

    public static int findSmallestElement(int[] arr) {
        if (arr == null || arr.length == 0) {
            throw new IllegalArgumentException("Array cannot be null or empty.");
        }

        int min = arr[0]; // Assume the first element is the smallest
        for (int i = 1; i < arr.length; i++) {
            if (arr[i] < min) {
                min = arr[i];
            }
        }
        return min;
    }

    public static void main(String[] args) {
        int[] arr1 = {10, 5, 20, 8, 15};
        System.out.println("Smallest element in arr1: " + findSmallestElement(arr1)); // 5

        int[] arr2 = {1, 5, 2};
        System.out.println("Smallest element in arr2: " + findSmallestElement(arr2)); // 1
    }
}
```

---

### Approach 2: Post-Java 8 (Functional Style - Using Streams)

**Thought Process:**
Java 8 Streams provide a concise way to find the minimum element using the `min()` terminal operation. This method takes a `Comparator` to define the comparison logic. For primitive types like `int`, `IntStream` offers a direct `min()` method.

**Implementation:**
```java
import java.util.Arrays;
import java.util.OptionalInt;

public class SmallestElementPostJava8 {

    public static int findSmallestElement(int[] arr) {
        if (arr == null || arr.length == 0) {
            throw new IllegalArgumentException("Array cannot be null or empty.");
        }

        OptionalInt min = Arrays.stream(arr)
                                .min();

        // OptionalInt.getAsInt() should only be called if isPresent() is true
        // Since we handle empty array above, it will always be present here.
        return min.getAsInt();
    }

    public static void main(String[] args) {
        int[] arr1 = {10, 5, 20, 8, 15};
        System.out.println("Smallest element in arr1: " + findSmallestElement(arr1)); // 5

        int[] arr2 = {1, 5, 2};
        System.out.println("Smallest element in arr2: " + findSmallestElement(arr2)); // 1
    }
}
```

---

### Discussion on Approaches:

- → **Readability:** Both approaches are quite readable. The imperative approach is explicit about the loop and comparison. The Stream API approach is more declarative and concise, especially for those familiar with streams.
- → **Conciseness:** The Stream API solution is more compact.
- → **Performance:** For primitive arrays, the imperative loop is often slightly more performant due to less overhead compared to stream creation and processing. However, for larger datasets or more complex operations, the Stream API (especially parallel streams) can offer benefits.
- → **Error Handling:** Both approaches handle null or empty arrays by throwing an `IllegalArgumentException`.
## 063.Java-coding-problem-8.md
## 📝 Coding Problem 8: Sum of All Elements in an Array

**Problem Statement:**
Given an array of integers, calculate the sum of all its elements.

**Example:**
Input: `int[] arr = {1, 2, 3, 4, 5};`
Output: `15`

---

### Approach 1: Pre-Java 8 (Imperative Style - Loop)

**Thought Process:**
The most straightforward way to sum elements in an array is to iterate through each element and add it to a running total. Initialize a sum variable to zero and accumulate the values.

**Implementation:**
```java
public class SumArrayPreJava8 {

    public static int sumArrayElements(int[] arr) {
        if (arr == null) {
            throw new IllegalArgumentException("Array cannot be null.");
        }

        int sum = 0;
        for (int element : arr) {
            sum += element;
        }
        return sum;
    }

    public static void main(String[] args) {
        int[] arr1 = {1, 2, 3, 4, 5};
        System.out.println("Sum of arr1 elements: " + sumArrayElements(arr1)); // 15

        int[] arr2 = {10, -5, 20};
        System.out.println("Sum of arr2 elements: " + sumArrayElements(arr2)); // 25

        int[] arr3 = {};
        System.out.println("Sum of arr3 elements: " + sumArrayElements(arr3)); // 0
    }
}
```

---

### Approach 2: Post-Java 8 (Functional Style - Using Streams)

**Thought Process:**
Java 8 Streams provide a direct way to sum elements using the `sum()` terminal operation on an `IntStream`. We can convert the array to an `IntStream` and then call `sum()`.

**Implementation:**
```java
import java.util.Arrays;

public class SumArrayPostJava8 {

    public static int sumArrayElements(int[] arr) {
        if (arr == null) {
            throw new IllegalArgumentException("Array cannot be null.");
        }

        return Arrays.stream(arr)
                     .sum();
    }

    public static void main(String[] args) {
        int[] arr1 = {1, 2, 3, 4, 5};
        System.out.println("Sum of arr1 elements: " + sumArrayElements(arr1)); // 15

        int[] arr2 = {10, -5, 20};
        System.out.println("Sum of arr2 elements: " + sumArrayElements(arr2)); // 25

        int[] arr3 = {};
        System.out.println("Sum of arr3 elements: " + sumArrayElements(arr3)); // 0
    }
}
```

---

### Discussion on Approaches:

- → **Readability:** Both approaches are highly readable. The imperative loop is explicit, while the Stream API approach is concise and declarative.
- → **Conciseness:** The Stream API solution is more compact.
- → **Performance:** For primitive arrays, the imperative loop is often slightly more performant due to less overhead compared to stream creation and processing. However, the difference is usually negligible for typical array sizes.
- → **Error Handling:** Both approaches handle null arrays. The Stream API's `sum()` method correctly returns 0 for an empty array, which is generally the desired behavior for a sum.
## 064.Java-coding-problem-9.md
## 📝 Coding Problem 9: Remove Duplicates from a List

**Problem Statement:**
Given a `List<String>`, return a new `List<String>` containing only the unique elements from the original list, preserving the original order of first appearance.

**Example:**
Input: `List<String> words = Arrays.asList("apple", "banana", "apple", "orange", "banana", "grape");`
Output: `["apple", "banana", "orange", "grape"]`

---

### Approach 1: Pre-Java 8 (Imperative Style - Using LinkedHashSet)

**Thought Process:**
The most efficient way to remove duplicates while preserving insertion order is to use a `LinkedHashSet`. A `LinkedHashSet` maintains the order of elements as they are inserted and does not allow duplicates. We can add all elements from the input list to a `LinkedHashSet` and then convert it back to a `List`.

**Implementation:**
```java
import java.util.ArrayList;
import java.util.LinkedHashSet;
import java.util.List;
import java.util.Set;
import java.util.Arrays;

public class RemoveDuplicatesPreJava8 {

    public static List<String> removeDuplicates(List<String> words) {
        if (words == null) {
            return new ArrayList<>(); // Or throw IllegalArgumentException
        }
        Set<String> uniqueWords = new LinkedHashSet<>(words);
        return new ArrayList<>(uniqueWords);
    }

    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "apple", "orange", "banana", "grape");
        List<String> uniqueWords = removeDuplicates(words);
        System.out.println("Unique Words (Pre-Java 8): " + uniqueWords);

        List<String> emptyList = Arrays.asList();
        System.out.println("Unique Words (Empty List): " + removeDuplicates(emptyList));
    }
}
```

---

### Approach 2: Post-Java 8 (Functional Style - Using Streams)

**Thought Process:**
Java 8 Streams provide a very concise way to remove duplicates using the `distinct()` intermediate operation. This operation returns a stream consisting of the distinct elements (according to `equals()` and `hashCode()`). We then collect the distinct elements into a new list.

**Implementation:**
```java
import java.util.List;
import java.util.Arrays;
import java.util.stream.Collectors;

public class RemoveDuplicatesPostJava8 {

    public static List<String> removeDuplicates(List<String> words) {
        if (words == null) {
            return List.of(); // Returns an immutable empty list
        }
        return words.stream()
                    .distinct() // Returns a stream with unique elements
                    .collect(Collectors.toList()); // Collects to a new List
    }

    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "apple", "orange", "banana", "grape");
        List<String> uniqueWords = removeDuplicates(words);
        System.out.println("Unique Words (Post-Java 8): " + uniqueWords);

        List<String> emptyList = Arrays.asList();
        System.out.println("Unique Words (Empty List): " + removeDuplicates(emptyList));
    }
}
```

---

### Discussion on Approaches:

- → **Readability:** Both approaches are highly readable. The `LinkedHashSet` approach is a well-known idiom for this problem. The Stream API's `distinct()` method is very expressive and concise.
- → **Conciseness:** The Stream API solution is more compact.
- → **Performance:** Both approaches have similar time complexities (typically O(N) on average, where N is the number of elements, due to hash set operations). The `LinkedHashSet` approach might be marginally faster in some scenarios due to less overhead than stream creation and collection. However, for most practical purposes, the difference is negligible.
- → **Order Preservation:** Both methods preserve the original insertion order of the unique elements.
- → **Immutability (Post-Java 8):** Note that `List.of()` (used for empty list in Post-Java 8 example) creates an immutable list. If a mutable list is required, `new ArrayList<>()` should be used with `Collectors.toCollection(ArrayList::new)`.
## 065.Java-coding-problem-10.md
## 📝 Coding Problem 10: Sort a List of Objects by a Property

**Problem Statement:**
Given a `List` of `Person` objects (each having `name` and `age` properties), sort the list by `age` in ascending order. If ages are the same, sort by `name` alphabetically.

**Example:**
Input: `List<Person> people = Arrays.asList(new Person("Alice", 30), new Person("Bob", 25), new Person("Charlie", 30));`
Output (sorted): `[Person{name='Bob', age=25}, Person{name='Alice', age=30}, Person{name='Charlie', age=30}]`

---

### Helper Class: `Person`
```java
public class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    @Override
    public String toString() {
        return "Person{'" + name + "', age=" + age + "}";
    }
}
```

---

### Approach 1: Pre-Java 8 (Imperative Style - Anonymous Inner Class Comparator)

**Thought Process:**
Before Java 8, sorting a custom object list required implementing the `Comparator` interface, typically using an anonymous inner class. The `Collections.sort()` method would then use this comparator to sort the list.

**Implementation:**
```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;
import java.util.Arrays;

public class SortPeoplePreJava8 {

    public static void sortPeople(List<Person> people) {
        Collections.sort(people, new Comparator<Person>() {
            @Override
            public int compare(Person p1, Person p2) {
                // Sort by age ascending
                int ageComparison = Integer.compare(p1.getAge(), p2.getAge());
                if (ageComparison != 0) {
                    return ageComparison;
                } else {
                    // If ages are same, sort by name alphabetically
                    return p1.getName().compareTo(p2.getName());
                }
            }
        });
    }

    public static void main(String[] args) {
        List<Person> people = new ArrayList<>(Arrays.asList(
            new Person("Alice", 30),
            new Person("Bob", 25),
            new Person("Charlie", 30),
            new Person("David", 25)
        ));

        System.out.println("Original: " + people);
        sortPeople(people);
        System.out.println("Sorted (Pre-Java 8): " + people);
    }
}
```

---

### Approach 2: Post-Java 8 (Functional Style - Lambda and Comparator.comparing)

**Thought Process:**
Java 8 significantly simplifies sorting with lambda expressions and the `Comparator.comparing()` and `thenComparing()` methods. This allows for much more concise and readable sorting logic.

**Implementation:**
```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;
import java.util.Arrays;

public class SortPeoplePostJava8 {

    public static void sortPeople(List<Person> people) {
        // Sort by age, then by name
        Collections.sort(people, Comparator.comparing(Person::getAge)
                                          .thenComparing(Person::getName));
    }

    public static void main(String[] args) {
        List<Person> people = new ArrayList<>(Arrays.asList(
            new Person("Alice", 30),
            new Person("Bob", 25),
            new Person("Charlie", 30),
            new Person("David", 25)
        ));

        System.out.println("Original: " + people);
        sortPeople(people);
        System.out.println("Sorted (Post-Java 8): " + people);
    }
}
```

---

### Discussion on Approaches:

- → **Readability & Conciseness:** The Java 8 approach is vastly superior in terms of readability and conciseness. Lambda expressions and method references make the sorting logic much clearer and less verbose than anonymous inner classes.
- → **Maintainability:** The Java 8 `Comparator.comparing()` and `thenComparing()` methods are highly composable, making it easy to add more sorting criteria if needed.
- → **Performance:** The underlying sorting algorithm (typically Timsort for `Collections.sort()`) remains the same. The performance difference between the two approaches is negligible, as it primarily affects the syntax of defining the comparator.
- → **Idiomatic Java:** The Java 8 approach is the modern and idiomatic way to sort collections of custom objects.
## 066.Java-coding-problem-11.md
## 📝 Coding Problem 11: Filter a List of Objects based on a Property

**Problem Statement:**
Given a `List` of `Person` objects (from Problem 10), filter out and return a new list containing only people older than a certain age.

**Example:**
Input: `List<Person> people = [Person("Alice", 30), Person("Bob", 25), Person("Charlie", 35)];`, `ageThreshold = 30`
Output: `[Person{name='Charlie', age=35}]`

---

### Helper Class: `Person` (Same as Problem 10)
```java
public class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    @Override
    public String toString() {
        return "Person{name='" + name + "', age=" + age + "}";
    }
}
```

---

### Approach 1: Pre-Java 8 (Imperative Style - Loop and new List)

**Thought Process:**
Before Java 8, filtering a list involved iterating through it, checking a condition for each element, and adding elements that satisfy the condition to a new list.

**Implementation:**
```java
import java.util.ArrayList;
import java.util.List;
import java.util.Arrays;

public class FilterPeoplePreJava8 {

    public static List<Person> filterPeopleByAge(List<Person> people, int ageThreshold) {
        List<Person> filteredPeople = new ArrayList<>();
        for (Person person : people) {
            if (person.getAge() > ageThreshold) {
                filteredPeople.add(person);
            }
        }
        return filteredPeople;
    }

    public static void main(String[] args) {
        List<Person> people = Arrays.asList(
            new Person("Alice", 30),
            new Person("Bob", 25),
            new Person("Charlie", 35),
            new Person("David", 40)
        );

        int ageThreshold = 30;
        List<Person> olderPeople = filterPeopleByAge(people, ageThreshold);
        System.out.println("People older than " + ageThreshold + " (Pre-Java 8): " + olderPeople);
    }
}
```

---

### Approach 2: Post-Java 8 (Functional Style - Stream `filter()`)

**Thought Process:**
Java 8's Stream API provides a `filter()` intermediate operation that takes a `Predicate` (a functional interface) as an argument. This allows for a very concise and declarative way to filter elements.

**Implementation:**
```java
import java.util.List;
import java.util.Arrays;
import java.util.stream.Collectors;

public class FilterPeoplePostJava8 {

    public static List<Person> filterPeopleByAge(List<Person> people, int ageThreshold) {
        return people.stream()
                    .filter(person -> person.getAge() > ageThreshold) // Filter using a lambda predicate
                    .collect(Collectors.toList()); // Collect the filtered elements into a new list
    }

    public static void main(String[] args) {
        List<Person> people = Arrays.asList(
            new Person("Alice", 30),
            new Person("Bob", 25),
            new Person("Charlie", 35),
            new Person("David", 40)
        );

        int ageThreshold = 30;
        List<Person> olderPeople = filterPeopleByAge(people, ageThreshold);
        System.out.println("People older than " + ageThreshold + " (Post-Java 8): " + olderPeople);
    }
}
```

---

### Discussion on Approaches:

- → **Readability & Conciseness:** The Java 8 Stream API approach is significantly more concise and often considered more readable for filtering operations, as it clearly expresses the intent (`filter`) rather than the mechanics of iteration.
- → **Maintainability:** The Stream API approach is highly maintainable. If the filtering logic becomes more complex, the lambda expression can be easily modified or replaced with a separate `Predicate` object.
- → **Performance:** For simple filtering, the performance difference between the two approaches is often negligible. The Stream API might have some overhead for very small lists, but for larger datasets, it can potentially leverage parallel processing (`parallelStream()`) for better performance.
- → **Immutability:** Both approaches return a new list, leaving the original list unchanged.
## 067.Java-coding-problem-12.md
## 📝 Coding Problem 12: Map a List of Objects to a List of their Properties

**Problem Statement:**
Given a `List` of `Person` objects (from Problem 10), extract and return a new list containing only the names of these people.

**Example:**
Input: `List<Person> people = [Person("Alice", 30), Person("Bob", 25), Person("Charlie", 35)];`
Output: `["Alice", "Bob", "Charlie"]`

---

### Helper Class: `Person` (Same as Problem 10)
```java
public class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    @Override
    public String toString() {
        return "Person{name='" + name + "', age=" + age + "}";
    }
}
```

---

### Approach 1: Pre-Java 8 (Imperative Style - Loop and new List)

**Thought Process:**
Before Java 8, transforming a list of objects into a list of one of their properties involved iterating through the original list, accessing the desired property for each object, and adding it to a new list.

**Implementation:**
```java
import java.util.ArrayList;
import java.util.List;
import java.util.Arrays;

public class MapPeopleNamesPreJava8 {

    public static List<String> getPersonNames(List<Person> people) {
        List<String> names = new ArrayList<>();
        for (Person person : people) {
            names.add(person.getName());
        }
        return names;
    }

    public static void main(String[] args) {
        List<Person> people = Arrays.asList(
            new Person("Alice", 30),
            new Person("Bob", 25),
            new Person("Charlie", 35)
        );

        List<String> names = getPersonNames(people);
        System.out.println("Person Names (Pre-Java 8): " + names);
    }
}
```

---

### Approach 2: Post-Java 8 (Functional Style - Stream `map()`)

**Thought Process:**
Java 8's Stream API provides the `map()` intermediate operation, which is specifically designed for transforming elements from one type to another. It takes a `Function` (a functional interface) as an argument, which defines how each element should be transformed.

**Implementation:**
```java
import java.util.List;
import java.util.Arrays;
import java.util.stream.Collectors;

public class MapPeopleNamesPostJava8 {

    public static List<String> getPersonNames(List<Person> people) {
        return people.stream()
                    .map(Person::getName) // Map each Person object to their name using a method reference
                    .collect(Collectors.toList()); // Collect the transformed elements into a new list
    }

    public static void main(String[] args) {
        List<Person> people = Arrays.asList(
            new Person("Alice", 30),
            new Person("Bob", 25),
            new Person("Charlie", 35)
        );

        List<String> names = getPersonNames(people);
        System.out.println("Person Names (Post-Java 8): " + names);
    }
}
```

---

### Discussion on Approaches:

- → **Readability & Conciseness:** The Java 8 Stream API approach is significantly more concise and often considered more readable for transformation operations. The use of a method reference (`Person::getName`) makes the intent very clear.
- → **Maintainability:** The Stream API approach is highly maintainable. If the transformation logic becomes more complex, the lambda expression or method reference can be easily modified.
- → **Performance:** For simple mapping, the performance difference between the two approaches is often negligible. The Stream API might have some overhead for very small lists, but for larger datasets, it can potentially leverage parallel processing (`parallelStream()`) for better performance.
- → **Immutability:** Both approaches return a new list, leaving the original list unchanged.
## 068.Java-coding-problem-13.md
## 📝 Coding Problem 13: Calculate Average of Numbers in a List

**Problem Statement:**
Given a `List<Integer>`, calculate the average of all numbers in the list.

**Example:**
Input: `List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);`
Output: `3.0`

---

### Approach 1: Pre-Java 8 (Imperative Style - Loop)

**Thought Process:**
To calculate the average, we need to sum all the elements and then divide by the count of elements. This involves iterating through the list, accumulating the sum, and keeping track of the count.

**Implementation:**
```java
import java.util.List;
import java.util.Arrays;

public class AveragePreJava8 {

    public static double calculateAverage(List<Integer> numbers) {
        if (numbers == null || numbers.isEmpty()) {
            return 0.0; // Or throw IllegalArgumentException
        }

        int sum = 0;
        for (int number : numbers) {
            sum += number;
        }
        return (double) sum / numbers.size();
    }

    public static void main(String[] args) {
        List<Integer> numbers1 = Arrays.asList(1, 2, 3, 4, 5);
        System.out.println("Average of numbers1: " + calculateAverage(numbers1)); // 3.0

        List<Integer> numbers2 = Arrays.asList(10, 20, 30);
        System.out.println("Average of numbers2: " + calculateAverage(numbers2)); // 20.0

        List<Integer> emptyList = Arrays.asList();
        System.out.println("Average of emptyList: " + calculateAverage(emptyList)); // 0.0
    }
}
```

---

### Approach 2: Post-Java 8 (Functional Style - Stream `average()`)

**Thought Process:**
Java 8 Streams provide a direct way to calculate the average using the `average()` terminal operation on an `IntStream` (or `DoubleStream`). We can convert the `List<Integer>` to an `IntStream` and then call `average()`.

**Implementation:**
```java
import java.util.List;
import java.util.Arrays;
import java.util.OptionalDouble;

public class AveragePostJava8 {

    public static double calculateAverage(List<Integer> numbers) {
        if (numbers == null || numbers.isEmpty()) {
            return 0.0; // Or throw IllegalArgumentException
        }

        OptionalDouble average = numbers.stream()
                                        .mapToInt(Integer::intValue) // Convert to IntStream
                                        .average(); // Calculate average

        return average.orElse(0.0); // Return 0.0 if the stream is empty
    }

    public static void main(String[] args) {
        List<Integer> numbers1 = Arrays.asList(1, 2, 3, 4, 5);
        System.out.println("Average of numbers1: " + calculateAverage(numbers1)); // 3.0

        List<Integer> numbers2 = Arrays.asList(10, 20, 30);
        System.out.println("Average of numbers2: " + calculateAverage(numbers2)); // 20.0

        List<Integer> emptyList = Arrays.asList();
        System.out.println("Average of emptyList: " + calculateAverage(emptyList)); // 0.0
    }
}
```

---

### Discussion on Approaches:

- → **Readability & Conciseness:** The Java 8 Stream API approach is significantly more concise and often considered more readable for aggregation operations like calculating averages. It clearly expresses the intent.
- → **Maintainability:** The Stream API approach is highly maintainable. The logic is encapsulated within the stream pipeline.
- → **Performance:** For simple average calculations, the performance difference is often negligible. The Stream API might have some overhead for very small lists. For very large datasets, `parallelStream()` could be used, but with careful consideration of overhead.
- → **Handling Empty List:** Both approaches handle empty lists gracefully, returning 0.0. The Stream API uses `OptionalDouble` to represent the possibility of an empty stream, which is a safer way to handle such cases than returning a magic number or throwing an exception.
## 069.Java-coding-problem-14.md
## 📝 Coding Problem 14: Group Objects by a Property

**Problem Statement:**
Given a `List` of `Person` objects (from Problem 10), group them by their `age`.

**Example:**
Input: `List<Person> people = [Person("Alice", 30), Person("Bob", 25), Person("Charlie", 30)];`
Output: `Map<Integer, List<Person>> = {25: [Person{name='Bob', age=25}], 30: [Person{name='Alice', age=30}, Person{name='Charlie', age=30}]}`

---

### Helper Class: `Person` (Same as Problem 10)
```java
public class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    @Override
    public String toString() {
        return "Person{name='" + name + "', age=" + age + "}";
    }
}
```

---

### Approach 1: Pre-Java 8 (Imperative Style - Loop and HashMap)

**Thought Process:**
Before Java 8, grouping objects involved iterating through the list, checking if the grouping key (in this case, `age`) already exists in a `HashMap`. If it does, add the current object to the existing list associated with that key. If not, create a new list, add the object, and put the new list into the map with the key.

**Implementation:**
```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Arrays;

public class GroupPeoplePreJava8 {

    public static Map<Integer, List<Person>> groupPeopleByAge(List<Person> people) {
        Map<Integer, List<Person>> peopleByAge = new HashMap<>();

        for (Person person : people) {
            int age = person.getAge();
            if (!peopleByAge.containsKey(age)) {
                peopleByAge.put(age, new ArrayList<>());
            }
            peopleByAge.get(age).add(person);
        }
        return peopleByAge;
    }

    public static void main(String[] args) {
        List<Person> people = Arrays.asList(
            new Person("Alice", 30),
            new Person("Bob", 25),
            new Person("Charlie", 30),
            new Person("David", 25)
        );

        Map<Integer, List<Person>> groupedPeople = groupPeopleByAge(people);
        System.out.println("Grouped People (Pre-Java 8): " + groupedPeople);
    }
}
```

---

### Approach 2: Post-Java 8 (Functional Style - Stream `Collectors.groupingBy()`)

**Thought Process:**
Java 8's Stream API, particularly `Collectors.groupingBy()`, is specifically designed for this kind of operation. It takes a `Function` to extract the grouping key and an optional downstream collector to process the values for each group.

**Implementation:**
```java
import java.util.List;
import java.util.Map;
import java.util.Arrays;
import java.util.stream.Collectors;

public class GroupPeoplePostJava8 {

    public static Map<Integer, List<Person>> groupPeopleByAge(List<Person> people) {
        return people.stream()
                    .collect(Collectors.groupingBy(Person::getAge)); // Group by age
    }

    public static void main(String[] args) {
        List<Person> people = Arrays.asList(
            new Person("Alice", 30),
            new Person("Bob", 25),
            new Person("Charlie", 30),
            new Person("David", 25)
        );

        Map<Integer, List<Person>> groupedPeople = groupPeopleByAge(people);
        System.out.println("Grouped People (Post-Java 8): " + groupedPeople);
    }
}
```

---

### Discussion on Approaches:

- → **Readability & Conciseness:** The Java 8 Stream API approach is significantly more concise and readable for grouping operations. `Collectors.groupingBy()` clearly expresses the intent of grouping.
- → **Maintainability:** The Stream API approach is highly maintainable. If the grouping logic changes, it's a simple modification to the `Function` passed to `groupingBy()`.
- → **Performance:** Both approaches have similar time complexities (typically O(N) where N is the number of elements). The Stream API might have some overhead for very small lists, but for larger datasets, it can potentially leverage parallel processing (`parallelStream()`) for better performance.
- → **Flexibility:** `Collectors.groupingBy()` is very flexible and can be combined with other collectors (e.g., `counting()`, `mapping()`) to perform more complex aggregations within each group.
## 070.Java-coding-problem-15.md
## 📝 Coding Problem 15: Partition a List of Objects based on a Property

**Problem Statement:**
Given a `List` of `Person` objects (from Problem 10), partition them into two groups: those older than a certain age, and those not older than that age. The output should be a `Map<Boolean, List<Person>>` where `true` maps to the list of people older than the threshold, and `false` maps to the list of people not older than the threshold.

**Example:**
Input: `List<Person> people = [Person("Alice", 30), Person("Bob", 25), Person("Charlie", 35)];`, `ageThreshold = 30`
Output: `Map<Boolean, List<Person>> = {true: [Person{name='Charlie', age=35}], false: [Person{name='Alice', age=30}, Person{name='Bob', age=25}]}`

---

### Helper Class: `Person` (Same as Problem 10)
```java
public class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    @Override
    public String toString() {
        return "Person{name='" + name + "', age=" + age + "}";
    }
}
```

---

### Approach 1: Pre-Java 8 (Imperative Style - Loop and Two Lists)

**Thought Process:**
To partition the list, we can iterate through each `Person` object. Based on the `ageThreshold`, we add the person to one of two separate `ArrayList`s: one for people older than the threshold and another for those not older. Finally, we store these two lists in a `HashMap` with `Boolean` keys (`true` for older, `false` for not older).

**Implementation:**
```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Arrays;

public class PartitionPeoplePreJava8 {

    public static Map<Boolean, List<Person>> partitionPeopleByAge(List<Person> people, int ageThreshold) {
        List<Person> olderPeople = new ArrayList<>();
        List<Person> notOlderPeople = new ArrayList<>();

        for (Person person : people) {
            if (person.getAge() > ageThreshold) {
                olderPeople.add(person);
            } else {
                notOlderPeople.add(person);
            }
        }

        Map<Boolean, List<Person>> partitionedMap = new HashMap<>();
        partitionedMap.put(true, olderPeople);
        partitionedMap.put(false, notOlderPeople);

        return partitionedMap;
    }

    public static void main(String[] args) {
        List<Person> people = Arrays.asList(
            new Person("Alice", 30),
            new Person("Bob", 25),
            new Person("Charlie", 35),
            new Person("David", 40),
            new Person("Eve", 30)
        );

        int ageThreshold = 30;
        Map<Boolean, List<Person>> partitioned = partitionPeopleByAge(people, ageThreshold);
        System.out.println("Partitioned People (Pre-Java 8): " + partitioned);
    }
}
```

---

### Approach 2: Post-Java 8 (Functional Style - Stream `Collectors.partitioningBy()`)

**Thought Process:**
Java 8's Stream API provides `Collectors.partitioningBy()`, which is specifically designed for this exact use case. It takes a `Predicate` as an argument and partitions the input elements into two groups based on whether the predicate returns `true` or `false` for each element.

**Implementation:**
```java
import java.util.List;
import java.util.Map;
import java.util.Arrays;
import java.util.stream.Collectors;

public class PartitionPeoplePostJava8 {

    public static Map<Boolean, List<Person>> partitionPeopleByAge(List<Person> people, int ageThreshold) {
        return people.stream()
                    .collect(Collectors.partitioningBy(person -> person.getAge() > ageThreshold));
    }

    public static void main(String[] args) {
        List<Person> people = Arrays.asList(
            new Person("Alice", 30),
            new Person("Bob", 25),
            new Person("Charlie", 35),
            new Person("David", 40),
            new Person("Eve", 30)
        );

        int ageThreshold = 30;
        Map<Boolean, List<Person>> partitioned = partitionPeopleByAge(people, ageThreshold);
        System.out.println("Partitioned People (Post-Java 8): " + partitioned);
    }
}
```

---

### Discussion on Approaches:

- → **Readability & Conciseness:** The Java 8 Stream API approach is exceptionally concise and readable. It clearly expresses the intent of partitioning the data based on a condition, making the code almost self-documenting.
- → **Maintainability:** The Stream API approach is highly maintainable. If the partitioning condition changes, only the `Predicate` needs to be updated.
- → **Performance:** Both approaches have similar time complexities (typically O(N) where N is the number of elements). The Stream API might have some overhead for very small lists, but for larger datasets, it can potentially leverage parallel processing (`parallelStream()`) for better performance.
- → **Idiomatic Java:** The Java 8 approach is the modern and idiomatic way to perform partitioning operations on collections.
