---
applyTo: "**/*.java"
description: "Java-specific backend add-on for the official standards and best practices for **Data Structure Selection** and **Thread Processing & Memory Safety**, aligned with modern Java guidelines. This file complements .github/instructions/java/backend.instructions.md and only adds Java-specific guidance not already covered there."
---

# Java Project Development Rules & Standards

## 1. Data-Structures Selection Guide

Choosing the correct data structure is critical for performance, thread safety, and memory efficiency. Follow these official guidelines:

### Arrays vs. Collections

* **Use Native Arrays (`T[]`, `int[]`, etc.)** when:
  * The size is strictly fixed and known in advance.
  * Performance and low memory overhead are critical.
  * Storing primitive types directly without boxing overhead (`int`, `long`, `boolean`, etc.).
* **Use Collections (`Collection`, `List`, `Set`, `Map`)** when:
  * The size is dynamic (grows or shrinks).
  * You need built-in utility methods (`add`, `remove`, `contains`, streams).
  * Storing Object types (wrapper classes required for primitives, e.g., `Integer`).

### Lists

* **`ArrayList`**: Default choice for general-purpose lists. Provides fast $O(1)$ random access by index and dynamic resizing. Ordered by insertion.
* **`LinkedList`**: Use only when frequent insertions and deletions at both ends are required (implements `Deque`). Higher memory overhead due to node pointers.
* **`CopyOnWriteArrayList`**: Use in multi-threaded environments where reads vastly outnumber writes. Thread-safe by cloning the underlying array on every mutation.
* **`Vector`**: **Legacy**. Avoid in new code; use `ArrayList` or `CopyOnWriteArrayList` instead.

### Sets

* **`HashSet`**: Unordered collection of unique elements backed by a hash table. Best for fast lookups and duplicate removal.
* **`TreeSet`**: Sorted set of unique elements maintained in natural ascending order or via a custom `Comparator` (backed by a Red-Black tree).
* **`LinkedHashSet`**: Maintains the insertion order while providing unique elements.
* **`CopyOnWriteArraySet` / `ConcurrentSkipListSet`**: Thread-safe alternatives for multi-threaded uniqueness and sorting requirements.

### Maps

* **`HashMap`**: Fastest general-purpose map for key-value lookups. Unordered; converts heavily collided buckets to Red-Black trees in Java 8+.
* **`TreeMap`**: Sorted map ordered by keys using natural ordering or a `Comparator` (Red-Black tree implementation).
* **`LinkedHashMap`**: Maintains insertion order. Useful for building caches (e.g., LRU cache).
* **`ConcurrentHashMap`**: **Mandatory** for multi-threaded, high-concurrency key-value operations. Never use raw `HashMap` across threads.
* **`Hashtable`**: **Legacy**. Avoid; use `ConcurrentHashMap` instead.

### Queues & Deques

* **`ArrayDeque`**: Recommended high-performance implementation for Queues (FIFO) and Stacks (LIFO). Replaces the legacy `Stack` class. **Not thread-safe**.
* **`ConcurrentLinkedQueue` / `ConcurrentLinkedDeque`**: Lock-free, thread-safe queues for high-throughput multi-threaded/event-driven architectures.
* **`ArrayBlockingQueue` / `LinkedBlockingQueue`**: Thread-safe blocking queues ideal for producer-consumer patterns with fixed or varying capacity.

---

## 2. Thread Processing & Concurrency Rules

Improper thread management and concurrency handling lead to race conditions, thread starvation, thread pool exhaustion, and severe memory leaks. Adhere strictly to the following rules:

### Avoiding Memory Leaks & Resource Leaks

* **Proper Executor Lifecycle Management**: Always shut down `ExecutorService` instances using `shutdown()` and `awaitTermination()`, or utilize modern try-with-resources structures (such as `Executors.newVirtualThreadPerTaskExecutor()`). Unclosed executors hold thread references and prevent garbage collection.
* **Clean Up ThreadLocals**: If using `ThreadLocal` variables within pooled threads (like fixed thread pools), always remove values in a `finally` block (`threadLocal.remove()`). Failing to do so causes stale data retention and permanent memory leaks across thread re-use.
* **Avoid Storing Large Object References**: Tasks submitted to long-lived threads or executors should avoid capturing enclosing class instances or large object graphs unless necessary, as long-lived references prevent GC collection of the heap.

### Thread Safety & Concurrency Best Practices

* **Use Concurrent Collections**: Never modify non-thread-safe collections (`ArrayList`, `HashMap`, `HashSet`) concurrently across multiple threads without explicit synchronization. Use `ConcurrentHashMap`, `CopyOnWriteArrayList`, or `BlockingQueue` instead.
* **Prefer Atomic Variables & Locks over Raw Synchronization**: Use `java.util.concurrent.atomic` classes (`AtomicInteger`, `AtomicLong`, `AtomicReference`) for simple shared counters/states, and `java.util.concurrent.locks.Lock` (`ReentrantLock`) for complex critical sections instead of error-prone manual `wait()`/`notify()` patterns.
* **Choose Appropriate Concurrency Utilities**:
  * Use `CountDownLatch` or `Phaser` to coordinate completion of multiple worker threads.
  * Use `Semaphore` to throttle resource access and prevent system overload.
  * Use `CompletableFuture` or reactive constructs (`WebClient`) for non-blocking asynchronous workflows rather than blocking on `Future.get()` unnecessarily.
* **Virtual Threads (Java 21+)**: Prefer `Executors.newVirtualThreadPerTaskExecutor()` for I/O-bound tasks to maximize throughput without manual thread pool sizing, but avoid pinning virtual threads by blocking on synchronized blocks/methods where native locks or `ReentrantLock` would be safer.

---

## 3. Memory Safety, Object Lifecycles, and Resource Handling

Java applications must be explicit about lifetime management and avoid accidental retention of unused objects.

* Prefer `try-with-resources` for every `InputStream`, `OutputStream`, `Reader`, `Writer`, `Connection`, `Statement`, `ResultSet`, and other `AutoCloseable` resources.
* Do not keep large collections, network clients, database sessions, or file handles longer than necessary.
* Release references promptly after use; avoid storing request-scoped or user-scoped objects in static fields or long-lived caches.
* Prefer bounded caches and explicit eviction policies instead of unbounded maps.
* Avoid repeated object creation in hot paths when the object is not necessary; favor pooling or reuse only when profiling justifies it.
* Treat `ThreadLocal` values as short-lived and always clean them up in `finally`.
* Use `Optional` for return values that may be absent, but avoid using it as a method parameter for required values.
* Prefer immutability for domain objects and configuration-bound data when possible to reduce shared-state risk.

---

## 4. Coding Standards for Java Quality and Safety

* Use strong typing and avoid raw types, unchecked casts, and ambiguous object models.
* Prefer `final` for local variables, parameters, and fields when the value should not change after initialization.
* Prefer records, enums, and sealed types for explicit, value-oriented models and restricted hierarchies.
* Avoid overly deep inheritance chains or abstraction layers that hide behavior instead of clarifying it.
* Keep methods short, deterministic, and focused on a single responsibility.
* Prefer defensive copies for mutable inputs when storing or exposing them outside a method boundary.
* Check for nulls early and handle invalid states explicitly; do not rely on hidden defaults in critical business logic.
* Avoid swallowing exceptions silently; either handle them meaningfully or rethrow with context.
* Never log sensitive data such as passwords, access tokens, private keys, personal information, or PII.

---

## 5. Exception Handling and Validation Rules

* Validate request payloads, identifiers, and configuration values at the boundary before business processing starts.
* Translate technical exceptions into clear domain or application exceptions at the service layer.
* Use checked exceptions only when recovery is genuinely expected; use unchecked exceptions for logic violations or unexpected runtime failures.
* Preserve useful error context without exposing internal stack traces to end users.
* Fail closed when critical validation fails rather than proceeding with incomplete or unsafe data.
* Use custom exceptions for domain violations, not generic `RuntimeException` values in production code.

---

## 6. Dependency and Framework Constraints

* Prefer standard Java libraries and well-supported frameworks over custom reimplementations.
* Keep business logic independent from infrastructure concerns; do not mix persistence logic, HTTP handling, and domain rules in one file.
* Use dependency injection to manage services, repositories, and configuration objects.
* Prefer stable, explicit constructor injection instead of field injection or hidden container lookups.
* Keep service dependencies minimal and clear; avoid constructor bloat or a service that handles unrelated responsibilities.
* When using Spring Boot or other frameworks, align with the framework conventions and use idiomatic configuration instead of custom anti-patterns.

---

## 7. Performance Guardrails for Java Development

* Avoid unbounded collections, nested loops with growing work, and repeated expensive computation in frequently called methods.
* Prefer streaming APIs or batching when processing large datasets, but avoid creating unnecessary intermediate collections.
* Use pagination, filtering, and efficient queries for large result sets rather than loading everything into memory.
* Benchmark and profile non-trivial changes before and after optimization work.
* Prefer the simplest correct data structure over an overly clever one; readability and maintainability remain important.
* For I/O-heavy workloads, consider virtual threads or non-blocking patterns when they fit the architecture.

---

## 8. Review Checklist for Java Changes

Every Java change should be checked against the following questions:

* Did I choose the most appropriate collection and concurrency model for this use case?
* Am I holding references or resources longer than needed?
* Is the code thread-safe under expected concurrency?
* Are validation and failure paths explicit and safe?
* Are I/O and object lifecycles properly closed and cleaned up?
* Is the implementation aligned with the project architecture and framework conventions?
* Are performance-sensitive sections measured and reasonable for the data size involved?
