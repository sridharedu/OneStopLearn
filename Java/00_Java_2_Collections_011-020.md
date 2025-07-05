## 011.Java-generics-and-type-erasure.md
💎 **Generics**
- → A feature introduced in Java 5 to provide type-safety for collections and classes.
- → Allows you to specify the type of objects that a collection can hold.

📜 **Before Generics (Pre-Java 5)**
- → Collections stored objects of any type, leading to potential `ClassCastException` at runtime.
```java
// No type safety. Can add any object.
List myList = new ArrayList();
myList.add(123); // An Integer
myList.add("John"); // A String - this is allowed, but may cause errors later.
```

✅ **With Generics**
- → Type is specified using angle brackets `<>`.
- → The compiler enforces type-safety at compile time, preventing incorrect types from being added.
```java
// Specifies that this list can only hold Integers.
List<Integer> myList = new ArrayList<>();
myList.add(123); // OK
// myList.add("John"); // Compile-time error!
```

🗑️ **Type Erasure**
- → The process where the compiler removes the generic type information after compilation.
- → The bytecode produced is similar to the pre-generics code (using `Object`).
- → **Why?** For backward compatibility. It ensures that code written before Java 5 can still run on newer JVMs.
- → This means generics in Java are a **compile-time** feature, not a runtime one.
## 012.Java-collection-types.md
📦 **Collection Interfaces**
- → **`List`**: An ordered collection that allows duplicate elements.
  - → `ArrayList`: Resizable array implementation. Fast for random access.
  - → `LinkedList`: Doubly-linked list implementation. Fast for insertions and deletions.
- → **`Set`**: A collection that does not allow duplicate elements.
  - → `HashSet`: Uses a hash table for storage. Offers best performance.
  - → `LinkedHashSet`: Maintains insertion order.
  - → `TreeSet`: A `SortedSet` implementation that stores elements in sorted order.
- → **`Queue`**: A collection used to hold elements prior to processing (First-In, First-Out - FIFO).
  - → `PriorityQueue`: Orders elements based on their natural order or a custom comparator.
- → **`BlockingQueue`**: A sub-interface of `Queue` that supports operations that wait for the queue to become non-empty when retrieving an element, and wait for space to become available in the queue when storing an element. Useful for producer-consumer patterns.
- → **`Map`**: An object that maps keys to values. Does not allow duplicate keys.
  - → `HashMap`: The most commonly used implementation. Does not guarantee order.
  - → `LinkedHashMap`: Maintains insertion order of keys.
  - → `TreeMap`: A `SortedMap` implementation that keeps entries sorted according to the natural ordering of its keys.
## 013.Java-arraylist-vs-linkedlist.md
↔️ **ArrayList vs. LinkedList**

- → **Internal Structure**
  - → `ArrayList`: Backed by a dynamic array.
  - → `LinkedList`: Implemented as a doubly-linked list with nodes pointing to `previous` and `next` elements.

- → **Insertion/Deletion**
  - → `ArrayList`: Can be slow (O(n)) for random insertions/deletions because it may require shifting elements in the underlying array.
  - → `LinkedList`: Fast (O(1)) for insertions/deletions once the position is known, as it only involves updating node pointers.

- → **Random Access**
  - → `ArrayList`: Very fast (O(1)) for random access using an index (`get(index)`).
  - → `LinkedList`: Slow (O(n)) for random access as it requires traversing the list from the beginning or end to find the element.

- → **Use Case**
  - → `ArrayList`: Best for read-intensive applications where you have few insertions or deletions.
  - → `LinkedList`: Best for write-intensive applications with frequent insertions and deletions.
## 014.Java-vector-vs-arraylist.md
⚖️ **Vector vs. ArrayList & Hashtable vs. HashMap**

- → **Legacy Collections**
  - → `Vector` and `Hashtable` are older, legacy classes from early Java versions.

- → **Synchronization**
  - → `Vector` & `Hashtable`: **Synchronized**. All of their public methods are marked `synchronized`. This means they are thread-safe out of the box.
  - → `ArrayList` & `HashMap`: **Not synchronized**. They are not thread-safe by default.

- → **Performance**
  - → The built-in synchronization of `Vector` and `Hashtable` introduces a performance overhead. If one thread is accessing a method, other threads must wait, which can slow down the application.
  - → `ArrayList` and `HashMap` offer better performance in single-threaded environments or when synchronization is handled externally.

- → **Modern Usage**
  - → For thread-safe operations, it's generally preferred to use `Collections.synchronizedList(new ArrayList<>())` or use concurrent collections like `CopyOnWriteArrayList` and `ConcurrentHashMap` instead of `Vector` and `Hashtable`.
## 015.Java-HashMap-vs-LinkedHashmap.md
↔️ **HashMap vs. LinkedHashMap**

- → **`HashMap`**
  - → Does **not** guarantee the order of elements.
  - → The iteration order can change over time as new elements are added.

- → **`LinkedHashMap`**
  - → Maintains the **insertion order** of elements.
  - → When you iterate over a `LinkedHashMap`, the elements will appear in the order in which they were added.
  - → It does this by maintaining a doubly-linked list connecting all of its entries.
## 016.Java-How-to-create-a-Generic-Class.md
🛠️ **Creating a Generic Class**

- → **1. Define the Class with a Type Parameter**
  - → Use angle brackets `<>` next to the class name to specify a generic type placeholder (e.g., `<T>`).
  - → By convention, `T` stands for Type, `E` for Element, `K` for Key, `V` for Value.

- → **2. Use the Type Parameter**
  - → Use the placeholder (`T`) within the class for:
    - → Field data types (`private T data;`)
    - → Method return types (`public T getData() { ... }`)
    - → Method or constructor parameters (`public void setData(T data) { ... }`)

- → **3. Instantiate the Generic Class**
  - → When creating an object, specify the concrete type you want to use.
  - → The compiler will replace the placeholder `T` with the specified type.

```java
// 1. Define the generic class
class Box<T> {
    private T content;

    public void setContent(T content) {
        this.content = content;
    }

    public T getContent() {
        return content;
    }
}

// 3. Instantiate with specific types
Box<Integer> integerBox = new Box<>();
integerBox.setContent(123);

Box<String> stringBox = new Box<>();
stringBox.setContent("Hello World");
```
## 017.Java-Failfast-vs-Failsafe-Iterators.md
⚡ **Fail-Fast vs. Fail-Safe Iterators**

- → **Fail-Fast Iterators**
  - → **Source:** Provided by traditional collection classes like `ArrayList`, `HashSet`, and `HashMap`.
  - → **Behavior:** Throws a `ConcurrentModificationException` if the collection is structurally modified (add, remove) after the iterator is created, by any means other than the iterator's own `remove()` method.
  - → **Environment:** This happens in both single-threaded and multi-threaded scenarios.
  - → **Goal:** To alert the developer about potential concurrency issues immediately.

- → **Fail-Safe Iterators**
  - → **Source:** Provided by concurrent collection classes like `CopyOnWriteArrayList` and `ConcurrentHashMap`.
  - → **Behavior:** Does **not** throw `ConcurrentModificationException`. They operate on a snapshot or a clone of the collection.
  - → **Trade-off:** The iterator might not reflect the most up-to-date state of the collection after it was created.
  - → **Goal:** To ensure iteration can complete without interruption, even if the underlying collection is being modified.
## 018.Java-Producer-Consumer-Pattern.md
🔄 **Producer-Consumer Pattern**

- → **Recommended Collection: `BlockingQueue`**
  - → The `BlockingQueue` interface is specifically designed to simplify the implementation of the producer-consumer problem.

- → **Producer Thread**
  - → Uses the `put(E e)` method to add elements (work) to the queue.
  - → If the queue is full, the `put()` method will **block** (wait) until space becomes available.

- → **Consumer Thread**
  - → Uses the `take()` method to retrieve elements from the queue.
  - → If the queue is empty, the `take()` method will **block** until an element is available.

- → **Benefit**
  - → `BlockingQueue` handles all the synchronization and waiting, making the logic for the producer and consumer very clean and straightforward.
## 019.Java-Comparable-vs-Comparator.md
⚖️ **Comparable vs. Comparator**

- → **`Comparable` Interface**
  - → **Purpose:** Provides a **natural** or **default** ordering for a class.
  - → **Package:** `java.lang`
  - → **Implementation:** The class whose objects you want to sort must implement this interface.
  - → **Method:** Requires implementing `int compareTo(T o)`.
  - → **Logic:** Defines the single, standard way to sort objects of that class.

- → **`Comparator` Interface**
  - → **Purpose:** Provides a **custom** or **external** ordering. You can define multiple different comparison strategies.
  - → **Package:** `java.util`
  - → **Implementation:** A separate class is created that implements this interface.
  - → **Method:** Requires implementing `int compare(T o1, T o2)`.
  - → **Logic:** Defines a specific ordering logic, independent of the class being sorted.

- → **Return Value Logic (for both)**
  - → **Negative integer:** `object1` should come before `object2`.
  - → **Zero:** `object1` and `object2` are equal.
  - → **Positive integer:** `object1` should come after `object2`.

- → **Usage in Collections (`TreeSet`, `TreeMap`)**
  - → If no `Comparator` is provided, the collection will look for the `Comparable` interface on the objects being added and use its `compareTo` method for sorting.
  - → If you pass a `Comparator` to the collection's constructor, it will use that `Comparator`'s `compare` method for sorting, overriding any natural ordering.
## 020.Java-concurrent-collections.md
⚡ **Concurrent Collections**

- → A set of collection classes designed specifically for use in multi-threaded environments.

🐢 **Traditional Collections (`ArrayList`, `HashMap`)**
- → When used in multi-threaded contexts (even when wrapped with `Collections.synchronized...`), they use a single lock for the entire object.
- → This means only one thread can access the collection at a time, causing performance bottlenecks.
- → They use **fail-fast iterators**, which throw a `ConcurrentModificationException` if the collection is modified while being iterated over.

🚀 **Concurrent Collections (The Solution)**

- → **`CopyOnWriteArrayList` / `CopyOnWriteArraySet`**
  - → **Strategy:** When a modification occurs, a fresh copy of the underlying array is made. All subsequent reads see the old copy until the write is complete.
  - → **Benefit:** Allows multiple threads to read the collection without any locking. Writes are expensive, but reads are fast and safe.
  - → **Iterators:** They provide **fail-safe iterators**. These iterators work on a snapshot of the collection at the time the iterator was created and do not throw `ConcurrentModificationException`.

- → **`ConcurrentHashMap`**
  - → **Strategy:** Implements a more sophisticated locking mechanism called **fine-grained locking** (or lock striping).
  - → **Benefit:** Instead of locking the entire map, it locks only the specific segment or "bucket" that is being modified. This allows multiple threads to write to different parts of the map simultaneously, greatly improving concurrency.
