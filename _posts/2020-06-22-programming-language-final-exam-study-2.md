---
layout: post
title:  "Programming language final exam study 2"
date:   2020-06-22
excerpt: "OOP"
tag:
- PL
- programming language
- 프로그래밍 언어
- blog
- silofox
comments: true
lastmod : 2020-06-22
sitemap : 
  changefreq : never
  priority : 1.0
feature: https://github.com/siloFoX/siloFoX.github.io/blob/master/images/programming-language/programming-language-feature.jpg?raw=true
---

# Chapter 1. Data Abstarction and Modularity

## Structured Programming

- Stepwise refinement : top-down or bottom-up
- Smaller and easier problems

<br>

### 1.1.1 Data Refinement

### 1.1.2 Modularity

- Divide-and-conquer
- Component : partially independent part of a program
    - Interface : A description of the parts of a compoonent that are visible to other prorgram components
    - Specification : A description of the behavior of a component, as observable through its interface

<br>

## 1.2 Language Support for Abstraction

- Client
- Implementation
    - Procedural abstraction
    - Data abstraction
    - Abstract data types vs. transparent type

<br>

# Chapter 2. Concepts in Object-Oriented Languages

- Object : a set of operations on hidden data

<br>

## 2.1 Object-Oriented Design

Iterative process

- Identify the objects at a given level of abstraction
- Identify the semantics (intended behavior) of these objects
- Identify the relationships among the objects
- Implement the objects

<br>

## 2.2 Four Basic Concepts in Object-Oriented Languages

- Dynamic lookup
- Abstraction : implementation details are hidden
- Subtyping : b is a subtype of a if a has all the functionality of b.
- Inheritance : ability to reuse the definition of one kind of object to define another kind of object.

<br>

### 2.2.1 Dynamic Lookup

- A method is selected dynamically according to the implementation of the object that receives a message.
- Example of graphic objects
- Implementation
    - Method lookup table
    - Dynamic overloading. The message recipient is regarded as the first argument. Multiple dispatch or single dispatch depending on the number of arguments used for method selection.
- Dynamic lookup is default for Java.
- Dynamic lookup is applied to only virtual functions in C++.

<br>

### 2.2.2 Abstraction

- Restricting access to a components according to its interface.
- Similar to data type, but more flexible.

<br>

### 2.2.3 Subtyping

- substitutivity
- Allows uniform operations on various types of data. For example, bank accounts.
- Functionality can be added without modifying general parts of a system.
- Polymorphism. For example, aircraft and control tower.

<br>

### 2.2.4 Inheritance

- Defining objects from existing ones.
- Implementation. Build linked structure of method lookup tables.
- View of abstraction with inheritance : Client(public), Implementation(private), and Class(protected).

<br>

### 2.2.5 Closure as Objects

- Similar to each other.
- Closure lacks in inheritance and subtyping.

<br>

### 2.2.6 Inheritance is not Subtyping

- Implementation vs. interface
- Example of Queue, Stack, Dequeue
    - Implement dequeue, and then implement queue and stack by restricting operations.
    - However, stack and queue are not subtype of dequeue.
    - Note that dequeue is a subtype of both stack and queue.

<br>

# Chapter 3. Objects and Run-Time Efficiency : C++

1984, by Bjarne Stroustrup at Bell Lab, inspired by Simula.

## 3.1 Design Goal and Constraints

- data abstraction and object-oriented features
- better static type checking
- backward compatibility with C
- efficiency of compiled code.

<br>

### 3.1.1 Compatibility with C

- The same data representation as C.
- Programmer-controlled memory management, not garbage-collected.
- Managing objects as if they were structures.

<br>

### 3.1.2 Success of C++

## 3.2 Overview of C++

### 3.2.1 Addictions to C Not Related to Objects

- type bool : Not a big change, but type bool standardized the representation.
- reference type and pass-by-reference

    ```c++
    void inc(int *p) { (*p)++; }
    main() {
        int k = 0;
        inc(&k);
        k;
        // ...
    }

    void inc (int& n) { n++; }
    main() {
        int k = 0;
        inc(k);
        k;
        // ...
    }
    ```

- user-defined overloading
    ```c++
    #include <stdio.h>

    void show (int val) { printf("%d\n", val); }
    void show (double val) { printf("Double: %lf\n", val); }
    void show (char *val) { printf("String: %s\n", val); }
    int main() {
        show(12);
        show(3.14);
        show("Hello, World!\n");
    }
    ```
Given
```c++
void f(int)
void f(int *)
```
Which one will be invoked for f('a')?

- function templates
- exceptions
- new/delete
- stream and file io
- default parameter values in function declarations

<br>

### 3.2.2 Object-Oriented Features

- Classes : declares type
- Objects :
- Dynamic lookup : virtual functions in derived classes
- Encapsulation : public, protected, private
- Inheritance : multiple inheritance allowed
- Subtyping :

<br>

### 3.2.3 Good Decisions and Problem Areas

- Encapsulation
- Separation of subtyping and inheritance : private or public base class
- templates
- exceptions
- better static type checking than C
- scope resolution operator (::) for single and multiple inheritance.

<b>Problem Area</b>

- casts and conversions
    - multiple inheritance
    - cast from subtype to supertype may change representation
- object allocated on the stack 
    - Note that Java allocated objects only on heap, meaning pointer access
    - Stack-allocated objects referenced throught stack variables
    - Assignment in combination with subtyping, truncating an object to an object
    - This may change behavior because it changes the way virtual functions are selected.
- overloading makes dynamic lookup complex : static behavior and dynamic behavior are mixed
- multiple inheritance and virtual functions.

<br>

## 3.3 Classes, Inheritance, and Virtual Functions

### 3.3.1 C++ Classes and Objects

```c++
class Pt {
public : 
    Pt (int xv);
    Pt (Pt *pv);
    int getX();
    virtual void move (int xv);
protected : 
    void setX (int xv);
private :
    int x;
};

Pt::Pt (int xv) { x = xv; }
Pt::Pt (Pt *pv) { x = pv->x; }

int Pt::getX() { return x; }
void Pt::setX (int xv) { x = xv; }
```

- Constructors
    - class method, not instance method
    - initializes the member data
- Visibility
    - public
    - protected
    - private
    - friend
- Virtual functions
    - Functions declared virtual can be redifined in the derived classes.
    - Dynamic selection, less efficient than non-virtual functions
    - Redclaration of non-virtual may give an overloaded functions

<br>

### 3.3.2 C++ Derived Classes (Inheritance)

```c++
class ColorPt : public Pt {
public : 
    ColorPt(int xv, int cv);
    ColorPt(Pt *pv, int cv);
    ColorPt(ColorPt *cp);
    int getColor();
    virtual void darken(int tint);
    virtual void moce(int dx);
protected :
    void setColor(int cv);
private :
    int color;
};

ColorPt::ColorPt(int xv, int cv) : Pt(xv) { color = cv; }
int ColorPt::getColor() { return color; }
void ColorPt::darken(int tint) { color += tint; }
void ColorPt::move(int dx) { Pt::Move(dx); this->darken(1); }
void ColorPt::setColor(int cv) { color = cv; }
```

- Inheritance
    - public base class Pt (default is private base class)
    - public base class makes its derived class its subtype.
- Constructors
- Visibility
- Virtual functions

<br>

### 3.3.3 Virtual Functions

- Each object has a pointer to a data structure associated with its class, called the virtual functions table, or vtable.
- Derived class has a copy of the base classes vtbl.
- Offsets in vtbl is determined at compile time.

<br>

## 3.4 Subtyping

### 3.4.1 Subtyping Principles

- Subtyping for classes : by public interface and public base class
```c++
Pt::int getX(); 
void move(int);
ColorPt::int getX();
int getColor();
void move(int);
void darken(int);
```

- Subtyping for functions :
```
(A → B) ⊆ (C → D) whenever C ⊆ A and B ⊆ D
```
Example
```
(circle → circle) ⊆ (circle → shape)
(shape → circle) ⊆ (circle → circle)
```

<br>

### 3.4.2 Public Base Classes

- Inheritance from public base class gives subtype relation
- Visibility and type should be maintained
- Structural subset relation does not hold in C++

<br>

### 3.4.3 Specializing Types of Puvlic Members

<b>Permissive inheritance</b> : Redifined public memeber of a derived class must have a subtype
<b>Contemporary C++ Inheritance</b> : virtual function f : A -> B may be redefined with f : A -> C, where C ⊆ B.
Argument types are used for resolving overloaded members.

<br>

### 3.4.4 Abstract Base Classes

- Organizational purpose
- Without implementation

```c++
class File {
public :
    void virtual Open() = 0;
    void virtual Read() = 0;
};
```

<br>

## Multiple Inheritance

### 3.5.1 Implementation of Multiple Inheritance

```c++
class A {
public : 
    int x;
    virtual void f();
};
class B {
public : 
    int y;
    virtual void g();
    virtual void f();
};
class C : public A, public B {
public : 
    int z;
    virtual void f();
};

C *pc = new C;
B *pb = pc;
A *pa = pc;
```

<b>Problems</b>

- Multiple virtual function tables for class C
- Pointer pb refers to a location different from those referred by the pointers pa and pc.
- C::f's

Note that

- Two vtables. In general, if there are n base classes, there would be n+1 vtables in the derived class.
- Arrangement of data members need to manage offset 

<br>

### 3.5.2 Name Clashes, Diamond Inheritance, and Virtual Base Classes

<b>Name clashes</b>

- Implicit resolution
- Explicit resolution : C++'s way
- Disallow name clashes

```c++
class A {
public :
    virtual void f() { ... }
};
class B {
public : 
    virtual void f() { ... }
};
class C : public A, public B {
public :
    virtual void f() {
        A::f();
    }
    // ...
};
```

<b>Diamond Inheritance problem</b> A simple scheme

<b>Virtual Base Classes</b>

- Using indirection
- Derived class has pointers accessing the members of its base class

<br>

# Chapter 4. Portability and Safty : Java

- Original name : Oak
- Purpose : set-top box programming
- By James Gosling at Sun Microsystems in early 1990's.
- Portablility : moving on network
- Safety : without fear of security breach.
- Main parts of Java
    - Java programming languages
    - Java compilers and run-time systems JVM
    - Extensive libraries

<br>

## 4.1 Java language Overview

## 4.1.1 Java Language Goals

- Portability
- Reliability
- Safety
- Dynamic Linking
- Multithreaded Execution
- Simplicity and Familiarity
- Efficiency

<br>

### 4.1.2 Design Decision

||Portability|Safety|Simplicity|Efficiency|
|:-:|:-:|:-:|:-:|:-:|
|Interpreted|+|+|||
|Type safe|+|+|+/-|+/-|
|Most values are object|+/-|+/-|+|-|
|Object by means of pointers|+||+|-|
|Garbage collection|+|+|+|-|
|Concurrency support|+|+|||
{: rules="groups"}

<br>

- Type safety at 3 levels : source, bytecode, execution time
- Primitive type by value, objects by reference
- Dynamic linking - incremental loading, short latency
- Simplicity : features not appearing in Java but in C++
    - Structures and unions. Union by superclass
    - Functions. Java has static method
    - Multiple inheritance. Interface in Java
    - Goto
    - Complex operator overloading
    - Automatic coercion
    - Pointers

<br>

## 4.2 Java Classes and Inheritance

### 4.2.1 Classes and Objects

```java
class Point {
public int getX() { ... }
protected void setX(int x) { ... }
private int x;
    Point(int xval) { x = xval; }
};
```

- Initialization
- Static fields and methods. Static initialization block.

    ```java
    class ... {
        static int x = initial_value;
        static { ... }
    };
    ```

- Overloading. Name resolution by using signatures.
- Garbage collection
- Finalize method : called by garbage collector and virtual machine when exit

    ```java
    class ... {
        ...
    protected : void finalize() {
        super.finalize();
        close(file);
        }
    };
    ```

- Uncaught exceptions raised while a finalize method is executed are ignored.
- Programmers have no control over finalize method
- main method
- toString method
- Native method

<br>

### 4.2.2 Packages and Visiability

- public 
- protected
- private
- package

Names declared in another package can be accessed with

- import
- qualified names : java.lang.Sting.substring()

<br>

### 4.2.3 Inheritance

subclass and superclass

```java
class ColorPoint extends Point {
    private Color c;
    protected void setC(color d) { c = d; }
    public Color getC() { return c; }
    ColorPoint(int xval, Color cval) {
        super(xval);
        c = cval;
    }
}
```

Method overriding and Field Hiding

- A method with the same signature
- Caution : return type conflict
- Fields are hidden. Can be accessed with qualified names, type cast to superclass, or key word super.

Constructor

- Automatic insertion of the default superclass constructor as the first statement
- ColorPoint() { ColorPoint(0, blue); }

Final Methods and Classess : inheritance or overriding is not allowed.

- singleton pattern
- java.lang.System
- Opposite to virtual in C++

Class Object is the root of all classes.

- Implicit, no need to specify
- Methods
    - getClass()
    - toString()
    - equals()
    - hashCode()
    - clone()
    - wait(), notify(), notifyAll()
    - finalize()

<br>

### 4.2.4 Abstract Classes and Interfaces

- No instance

    ```java
    abstract class Shape {
        ...
        abstract Point center();
        abstract void rotate(degrees d);
        ...
    }
    ```

- interface : pure abstract class

    ```java
    interface Shape {
        public Point center();
        public void rotate (float degrees);
    }
    interface Drawable {
        public void setColor (Color c);
        public void draw();
    }

    class Circle implements Shape, Drawable {
        // must define Shape, Drawable methods
    }
    ```

- Multiple inheritance
- Subtyping
- No name clash. Explain how to implement two names, one from each  interface, with the same or different signatures.

<br>

## 4.3 Java Types and Subtyping

Classification

- Primitive types : boolean, byte, short, int, long, char, float, double
- Reference types : array, class, interface

Subtyping for Classes and Interfaces

- Run-time type conversion : automatic conversion to supertype
- Implementation : member lookup for classes is different from that for interfaces

<br>

### 4.3.1 Arrays, Covariance, and Contravariance

For every type T, Java has an array type T[] Array types are final.

```java
Circle[] x = new Circle[array_size];
new int[] {1, 2, ... , 10 };
```

Array covariance problem

```java
class A { ... }
class B extends A { ... }
B[] bArray = new B[10];
A[] aArray = new bArray;
aArray[0] = new A();    // run-time type error
ArrayStoreException
```

<br>

### 4.3.2 Java Exception Class Hierarchy

A Java exception halts all on-going operations.<br>
Exception is represented by an object.<br>
Exception works in multithread environment.<br>
try-catch-finally block.<br>
Exception hierachy<br>
Show Java API for Exceptions

<br>

### 4.3.3 Subtype Polymorphism and Generic Programming

Comparison between subtype polymorphism and parametric polymorphism.

```java
class Node {
    Object element;
    Node next;
    Node (Node n, Object e) { next = n; element = e; }
}

public class Stack {
    private Node top = null;
    public Stack() {}
    public boolean empty() { return top == null; }
    public void push(Object val) { top = new Node(top, val); }
    public Object pop() {
        if (top == null) return -1;
        Node temp = top
        top = top.next;
        return temp.element;
    }
}

String s = "Hello";
Stack st = new Stack();

...
st.push(s);
...
s = (String) st.pop();

class Stack<A> {
    public boolean empty() { ... }
    public void push(A a) { ... }
    public A pop() { ... }
}

String s = "Hello";
Stack<String> st = new Stack<String>();
st.push(s);
...
s = st.pop();
```

Performance depends on run-time type checking.

<br>

## 4.4 Java System Architecture

### 4.4.1 Class Loader

```java
class LoadMyClass extends ClassLoader {
    private Hashtable loadedClasses = new Hashtable();
    public Class loadClass(String name, boolean resolve) {
        /* when resolve is true, preloading is preformed */
    }
}
```

- Incremental loading
- Define an alternative ClassLoader for remote class loading at runtime.

<br>

### 4.4.2 Java Linker, Verifier, and Type Discipline

Linker adds classes and interfaces to JVM's runtime system. Linker also initialized static fields.

- Opcode
- Branch target : prevent JOP
- Signatures of methods
- Types

May throw <b>VerifyError</b>.<br>
Linking

- Creating static fields and intializing them
- Name resolution
- Replacing dereferencing for direct referencing

GC may unload classes not uesd.

<br>

### 4.4.3 Bytecode Interpreter and Method Lookup

- Runtime checks : eg array bounds
- Stack machine : Each thread has a stack and program counter.
- Objects and arrays in the heap
- Activation records : local variables, operand stack (stack-within-a-stack), data area (constant pool, normal method return data, exception dispatch data)

Constant pool is a table of symbolic names,

- classes
- fields
- methods

that are referred by indices into the table.

Runtime tests

- Type cast
- Array bounds
- Non-null dereferencing

JVM's runtime optization on-the-fly : quick bytecodes refers the absolute address of a field or method.

```
getfield #18<Field Obj var>
=>
getfield_quick 6
```

where the involved object is at the top of the stack

<b>Finding a Virtual Method by Class</b>

- invokevirtual
- invokeinterface
- invokestatic
- invokespecial

Bytecode rewritiing on-the-fly

<b>Finding Virtual Method by Interface</b>

<br>

## 4.5 Security Features

CIA<br>
Mobile code :

- sandboxing
- code signing

### 4.5.1 Buffer Overflow Attack

```c++
void f(char *str) {
    char buffer[16];
    ...
    strcpy(buffer, str);
}

int main() {
    char large_string[256];
    int i;
    for (i = 0; i < 255; i++) large_string[i] = 'A';
    large_string[255] = 0;
    f(large_string);
}
```

<br>

### 4.5.2 The Java Sandbox

Four parties are involved in the Java's Sandboxing : class loader, Java bytecode verifier, run-time checks by JVM, security manager.

<b>Class Loader</b>

- Separates trusted classes from untrusted ones
- Separates names spaces from each other depending on class loaders used for class loading.
- Protection domain : places for code by which security manager restricts actions.

<b>The Bytecode Verifier and Virtual Machine Run-Time Tests</b>

- No stack overflow or underflow
- Type correctness of method calls
- No illegal casting
- Checking visibility restriction.

<b>Security Manager</b> Protection domains

- singer
- location : URL of the origin

Not monitoring, but consulting

- API should ask security manager when performing dangerous operations
- If the operation is not permitted, security manager throws a SecurityException. This goes back to the program

<br>

### 4.5.3 Security and Type Safety

<b>Casting int to a function pointer</b>

```c++
int (*fp)();
...
fp  = addr;
(*fp)();
```