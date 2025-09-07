# Core Java Interview Questions - Part 1
## (For Experienced Developers)

## Q1. Difference between JDK, JRE, and JVM
Core Answer:
- JVM: The abstract machine that executes bytecode.
- JRE: JVM + standard libraries (runtime only).
- JDK: JRE + development tools (compiler, debugger, jlink, jdeps).

Deep Explanation:
- JVM is a specification (e.g., HotSpot is one implementation).
- JRE historically was a separate download; now modern JDK distributions bundle all.
- With Java modules (Java 9+), you can create custom runtimes (jlink).
- Interviewers expect you to mention portability and the role of bytecode in WORA (Write Once Run Anywhere).

---

## Q2. Is Java 100% Object-Oriented?
Quick Answer: No—primitives are not objects.

Depth:
- Primitives (int, boolean, double…) don’t have methods and aren’t instances of classes.
- Wrapper types (Integer, Boolean, etc.) bridge the gap.
- Autoboxing makes it look objecty but has performance and semantic differences.
 

---

## Q3. Explain public static void main(String[] args)
Breakdown:
- public: Required so JVM can call it via reflection.
- static: No object needed.
- void: No return value expected; exit codes via System.exit().
- String[] args: CLI arguments.
 
---

## Q4. == vs equals()
Core:
- == : reference comparison for objects (or value compare for primitives).
- equals(): logical equality (may be overridden).
```java
String a = new String("test");
String b = new String("test");
System.out.println(a == b); // false (different refs)
System.out.println(a.equals(b)); // true (same content)

Integer x = 128, y = 128;
Integer m = 100, n = 100;
System.out.println(m == n); // true (within cache range -128 to 127)
System.out.println(m.equals(n)); // true
System.out.println(x == y); // false (outside cache range)
System.out.println(x.equals(y)); // true

```
---

## Q5. Autoboxing / Unboxing
Definition: Automatic conversion between primitive and wrapper.

Performance Impact:
- Boxing creates objects—can hurt GC in loops.
- Null unboxing throws NullPointerException.
```java
List<Integer> list = new ArrayList<>();
for (int i = 0; i < 1000; i++) {
    list.add(i); // Autoboxing
}

int sum = 0;
for (Integer val : list) {
    sum += val; // Unboxing
}

``` 

---

## Q6. Default Values of Instance Variables
Defaults (instance members):
- int: 0
- long: 0L
- boolean: false
- char: '\u0000'
- Object refs: null

Local variables: MUST be explicitly initialized.

---

## Q7. Pass-by-Value or Reference?
Java is strictly pass-by-value. For object parameters, the value passed is the reference (pointer) copy.
But here’s the trick:

When you pass a primitive, the actual value is copied.

When you pass an object, the value of the reference (memory address) is copied — not the object itself.
So both variables point to the same object, but the reference itself is a copy.

That’s why people sometimes get confused and think Java is pass-by-reference.

📌 Example 1: Primitives (Pass-by-Value)

``` java

public class PassByValue {
    public static void changeValue(int x) {
    x = 50;
}

    public static void main(String[] args) {
        int a = 10;
        changeValue(a);
        System.out.println(a); // 10 (not changed)
    }
}
```

Example: 2: Objects (Reference Value Copy)
```java
void mutate(StringBuilder sb){ sb.append("X"); }
void reassign(StringBuilder sb){ sb = new StringBuilder("New"); }

StringBuilder a = new StringBuilder("A");
mutate(a);       // a -> "AX"
reassign(a);     // a still "AX"

```
Pitfall: Saying “Java is pass-by-reference.”  

---
## Q8. String vs StringBuilder vs StringBuffer
- String: Immutable, thread-safe (final), slower for concatenation.
- StringBuilder: Mutable, not thread-safe, faster for single-threaded.
- StringBuffer: Mutable, thread-safe (synchronized), slower due to locking.
- Use StringBuilder for most cases unless thread safety is needed.
- String literals are interned (pooled) for memory efficiency.
- StringBuilder and StringBuffer have similar APIs; prefer StringBuilder unless synchronization is required.
- String concatenation with + operator creates new String objects (inefficient in loops).
```java
String s = "Hello";
s = s + " World"; // creates new String object
StringBuilder sb = new StringBuilder("Hello");
sb.append(" World"); // modifies existing object
```

---

## Q9. What is a Package?
Purpose: Namespace control, prevents naming conflicts, access control (package-private visibility).
- Declared at the top of the file: `package com.example;`
- Directory structure must match package name.
- Use `import` to bring in classes from other packages.
```java
package com.example;
import java.util.List;
public class MyClass { ... }
```

---

## Q10. Four Pillars of OOP
List: Encapsulation, Inheritance, Polymorphism, Abstraction.

Short Clarifications:
- Encapsulation: Hide data behind methods.
- Inheritance: Reuse via extends (use sparingly).
- Polymorphism: Runtime dispatch based on actual type.
- Abstraction: Focus on essential behavior (interfaces/abstract classes).

  Method Overloading vs Overriding
- Overloading: Same name, different parameter list (compile-time resolution).
```java
class Demo {
    void add(int a, int b) { ...}

    void add(double a, double b) { ...}
}
```
- Overriding: Same signature in subclass, runtime dispatch.
```java
class Parent {
    void show() { ... }
}
class Child extends Parent {
    @Override
    void show() { ... } // overrides Parent's show
}
```
---

## Q11. Can Static Methods Be Overridden?
Answer: No. They are hidden (shadowed). Binding is compile-time.
        
```java

class Parent {
    static void display() { System.out.println("Parent"); }
}
class Child extends Parent {
    static void display() { System.out.println("Child"); }
}
```
 

---

## Q12. Can Constructors Be Overridden?
Answer: No. They are not inherited. They can be overloaded.
```java
class Parent {
    Parent() { System.out.println("Parent Constructor"); }
}

class Child extends Parent {
    Child() {
        super(); // Calls Parent's constructor
        System.out.println("Child Constructor");
    }
}
```

---

## Q13. this vs super
this: Refers to current instance.  
super: Refers to immediate parent implementation / constructor.

```java
class Parent {
    int x = 10;
    void show() { System.out.println("Parent"+this.x); }
}
class Child extends Parent {
    int x = 20;
    void show() {
        System.out.println("Child"+this.x); // Child's x
        System.out.println("Parent"+super.x); // Parent's x
        super.show(); // Calls Parent's show()
    }
}
public class Test {
    public static void main(String[] args) {
        Child c = new Child();
        c.show();
    }
}
// Output:
// Child20
// Parent10
```

## Q14. Designing an Immutable Class
Rules:
- Declare class final (optional but recommended).
- All fields private & final.
- No setters.
- Defensive copies for mutable inputs & outputs.
- Ensure ‘this’ not leaked during construction.

```java
 public final class ImmutablePerson {
    private final String name;
    private final int age;

    ImmutablePerson(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String name() {
        return name;
    }

    public int age() {
        return age;
    }
}
```


 
