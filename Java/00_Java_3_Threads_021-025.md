## 021.Java-how-to-create-threads.md
🧵 **Creating Threads**

- → **1. Extend the `Thread` Class**
  - → Create a class that `extends` `java.lang.Thread`.
  - → Override the `run()` method and place the thread's execution logic inside it.
  - → **Limitation:** Since Java does not support multiple inheritance, your class cannot extend any other class.
  - → **Usage:** Create an instance of your class and call its `start()` method.

- → **2. Implement the `Runnable` Interface**
  - → Create a class that `implements` `java.lang.Runnable`.
  - → Provide the implementation for the `run()` method.
  - → **Advantage:** This is the preferred approach as it allows your class to extend another class if needed.
  - → **Usage:** 
    - → Create an instance of your runnable class.
    - → Create an instance of the `Thread` class, passing your runnable object to its constructor.
    - → Call the `start()` method on the `Thread` object.
## 022.Java-what-is-synchronization.md
🔒 **Thread Synchronization**

- → **Problem:** When multiple threads access shared data, they can interfere with each other, leading to data corruption.
- → **Solution:** Use the `synchronized` keyword to ensure that only one thread can execute a particular block of code at a time.

- → **Synchronized Methods**
  - → When a thread enters a method marked `synchronized`, it acquires an intrinsic lock on that object.
  - → No other thread can enter *any* `synchronized` method on the *same object* until the first thread exits the method and releases the lock.

- → **Other Uses**
  - → `synchronized` can also be applied to `static` methods and as a statement block (`synchronized(this) { ... }`).
## 023.Java-what-are-class-level-locks.md
🏛️ **Class-Level Locks**

- → Every class in Java has a unique lock associated with its `Class` object.
- → When a thread executes a `synchronized static` method, it acquires this **class-level lock**.
- → While this lock is held, no other thread can enter *any other* `synchronized static` method of the same class.
- → This is different from an object-level lock, which only locks the specific instance of the class.
## 024.Java-what-are-synchronized-blocks.md
🧱 **Synchronized Blocks**

- → **Problem:** Marking an entire method as `synchronized` can be inefficient if only a small part of the method needs protection.
- → **Solution:** Use a `synchronized` block to lock only the critical section of code.
- → **Syntax:** `synchronized(object) { // code to be synchronized }`
- → **Benefit:** This allows for more fine-grained control over locking, improving concurrency as other threads can still execute the non-synchronized parts of the method.
- → You can specify which object to lock on (e.g., `this` for the current object, or `MyClass.class` for a class-level lock).
## 025.Java-how-do-threads-communicate.md
🗣️ **Inter-Thread Communication**

- → Threads communicate using the `wait()`, `notify()`, and `notifyAll()` methods, which are part of the `Object` class.

- → **`wait()`**
  - → When a thread calls `wait()` on an object, it releases the lock on that object and enters a waiting state.

- → **`notify()`**
  - → A thread that holds the lock on the same object can call `notify()` to wake up a single waiting thread.

- → **`notifyAll()`**
  - → Wakes up *all* threads that are waiting on the object's lock.

- → **Requirement:** These methods **must** be called from within a `synchronized` method or block. If not, an `IllegalMonitorStateException` will be thrown.
