# JavaPreparation
**Day 1: Functional Programming & Lambdas (Java 8–11)**
Core Concepts: Functional Interfaces (@FunctionalInterface), Lambda syntax, and anonymous inner classes.
Built-in Interfaces: Master Predicate<T>, Function<T,R>, Consumer<T>, and Supplier<T>.
The Optional Class: Avoid NullPointerException. Learn .map(), .flatMap(), .orElse(), and .orElseThrow().

**why funcational programming..?**
Prior to Java 8, Java was strictly an Imperative and Object-Oriented Programming (OOP) language. You had to tell the computer how to do things step-by-step using loops, conditional blocks, and state mutations.
Functional Programming (FP) introduces a Declarative style. You tell the computer what you want to achieve, treating computation as the evaluation of mathematical functions while avoiding state mutation and mutable data.

# Day 1: Deep Dive – Functional Programming & Lambda Expressions

### The Problem solved by Java 8:
* **Boilerplate Reduction:** Passing a piece of behavior (logic) used to require wrapping it inside a heavy anonymous inner class.
* **Treating Code as Data:** FP allows you to pass functions (behavior) as arguments to methods or return them from methods (**First-Class Functions**).

---

## 2. Lambda Expressions: The Concept

### Simple Definition
A Lambda expression is an anonymous (nameless) method that provides a shortcut implementation of a method defined in a Functional Interface. It does not have an access modifier, a return type declaration, or a name.

```java
// 1. Classic Anonymous Inner Class Approach (Heavy Boilerplate)
Runnable r1 = new Runnable() {
    @Override
    public void run() {
        System.out.println("Running classic!");
    }
};

// 2. Modern Lambda Expression Approach (Clean & Single-line)
Runnable r2 = () -> System.out.println("Running modern!");
```

### Syntax Breakdown `() -> ...`
* **The Arguments `()`**: Represents the parameters of the method. If the method takes no arguments, use empty brackets. If it takes exactly one argument, the brackets can be omitted (e.g., `name -> ...`).
* **The Arrow `->`**: Connects the input parameters to the actual business logic body.
* **The Body**: The execution code block. Use curly braces `{}` and an explicit `return` statement if your logic spans multiple lines.

---

## 3. Core Mechanics & Under-the-Hood Design

### The Target: Functional Interfaces
You cannot use a Lambda anywhere you want. A Lambda expression can **only** implement a **Functional Interface**.
* **Definition:** A Functional Interface is an interface that contains **exactly one abstract method** (also known as a Single Abstract Method or SAM interface).
* **Annotation:** The optional `@FunctionalInterface` annotation explicitly signals the compiler to trigger an error if a second abstract method is added.

### Critical Interview Trap: Lambda vs. Anonymous Inner Class
Are Lambdas just syntactic sugar for Anonymous Inner Classes? **No.** They are fundamentally different under the hood.


| Feature | Anonymous Inner Class | Lambda Expression |
| :--- | :--- | :--- |
| **Compilation File Output** | Generates a physical `.class` file at compile time (e.g., `Test$1.class`). | Does **not** generate a separate `.class` file on disk. |
| **Memory Allocation** | Allocates a brand-new object instance on the Heap memory every single time it evaluates. | Reuses a highly optimized, lightweight functional instance managed dynamically. |
| **The `this` Keyword Scope** | `this` points explicitly to the internal inner class instance itself. | `this` points directly to the enclosing outer class instance (Lexical Scoping). |
| **Underlying Mechanism** | Standard class instantiation. | Relies on the **`invokedynamic`** JVM instruction. |

### The `invokedynamic` Instruction Deep-Dive
Instead of generating boilerplate class files on disk that increase runtime loading times, the Java compiler drops an `invokedynamic` instruction into the bytecode. 

When execution hits this instruction at runtime, the JVM uses a bootstrap method (`LambdaMetafactory`) to dynamically generate a highly optimized functional object in memory. This strategy minimizes application memory footprints and allows the JVM to inline the code for faster execution.

---

## 4. Cheat Sheet: Interview Questions & Proven Defenses

### Q1: Can you use a Lambda expression to implement an interface with two abstract methods?
* **Defense:** No. Lambda expressions require a Functional Interface, which explicitly enforces a Single Abstract Method (SAM) contract so the compiler can safely map the lambda signature to the target method.

### Q2: Does writing 100 Lambda expressions generate 100 `.class` files?
* **Defense:** No. Unlike anonymous inner classes, lambdas do not generate physical `.class` files on disk. The Java compiler inserts `invokedynamic` instructions that dynamically build lightweight runtime objects, reducing disk storage and accelerating JVM class loading speeds.

### Q3: When you assign a Lambda to an interface template (e.g., `Runnable r = () -> {};`), are you creating an instance of an interface?
* **Defense:** No, you cannot instantiate an interface in Java. You are actually initializing a dynamically-generated, lightweight functional object that satisfies the interface's method signature contract.

---

## 5. My Personal Revision Progress
* [x] Core Concept of Functional Programming vs Imperative
* [x] Lambda Syntax Structures (No args, single arg, multiple args with blocks)
* [x] Distinction between Anonymous Inner Classes and Lambdas
* [x] Mastery of bytecode tracking via `invokedynamic`


**Functional Interface**
Think of these four interfaces as simple input-output machines:1. PREDICATE      [ Takes Input ]  -------->  [ Returns boolean ]  (Evaluates a condition)
2. FUNCTION       [ Takes Input ]  -------->  [ Returns Output  ]  (Transforms data)
3. CONSUMER       [ Takes Input ]  -------->  [ Returns nothing ]  (Processes/Prints data)
4. SUPPLIER       [ No Input    ]  -------->  [ Returns Output  ]  (Generates data)
