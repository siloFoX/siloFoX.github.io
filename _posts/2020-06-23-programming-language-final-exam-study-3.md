---
layout: post
title:  "Programming language final exam study 3"
date:   2020-06-23
excerpt: "JavaScript II"
tag:
- PL
- programming language
- 프로그래밍 언어
- blog
- silofox
comments: true
feature: https://github.com/siloFoX/siloFoX.github.io/blob/master/images/programming-language/programming-language-feature.jpg?raw=true
---

# Chapter 14 JavaScript II

### Objects

An object is a collection of named properties

- Simplistic view in some documentation : hash table or associative array
- Can define by set of name : value pairs
    - objBob = { name : "Bob", grade : 'A', level : 3 };
- New properties can be added at any time<br>
objBob.fullname = 'Robert';
- Can have methods, can refer to this<br>
Not a common feature of hash tables or associative arrays
- Arrays, functions described in documentation as objects
    - A property of an object may be a function (=method)
    - A function defines an object with method called "()"<br>
    function max(x, y) if (x?y) return x; else return y;<br>
    max.description = return the maximum of two arguments;

<br>

### Basic object features

Use a function to construct an object

```js
function car(make, model, year) {
    this.make = make;
    this.model = model;
    this.year = year;
}
```

Objects have prototypes, can be changed

```js
let c = new car(Tesla, S, 2012);
car.prototype.print = function () {
    return this.year + this.make + this.model; 
}
c.print();
```

<br>

### this is a function

"this" in a function

```js
let o = { x : 10, f : function()  { return this.x }}
o.f();
```

Resolution of this is dynamic

Arrow functions don't have their own this. If we reference this from such a function, its taken from the outer normal function.

<br>

### Local variables in stack memory

Activation record

<br>

### Garbage collection

Automatic reclamation of unused memory

- Naviogator 2
    - per page memory management
    - Reclaim memory when browser changes page
- Navigator 3
    - reference counting
    - Each memory region has associated count
    - Count modified when pointers are changed
    - Reclaim memory when count reaches zero
- Navigator 4
    - mark-and-sweep, or eqivalent
    - Garbage collector marks reachable memory
    - Sweep and reclaim unreachable memory

<br>

### Closures

```js
function f(x) {
    let y = x;
    return function (z) { y += z; return y; }
}

let h = f(5);
h(3);
```

<br>

### Exceptions

Throw an expression of any type

```js
throw "Error2";
throw 42;
throw { toString : function() { return "I'm an object!"; }};
```

Catch

```js
try {
} catch (e if e == "FirstException") { // do something
} catch (e if e == "SecondException") { // do something else
} catch (e) { // executed if no match above 
}
```

<br>

### Object features

Dynamic lookup : Method depends on run-time value of object<br>
Encapsulation : Object contains private data, public operations<br>
Subtyping : Object of one type can be used in place of another<br>
Inheritance : Use implementation of one kind of object to implement another kind of object

<br>

### Concurrency

To be filled later

<br>

### 14.0.1 Eval function

Evaluate string as code
    The eval function evaluates a string of JavaScript code, 
    in scope of the calling code
Examples

```js 
let code = "let a = 1";
eval(code); // a is now '1
let obj = new Object();
obj.eval(code); // obj.a is now 1
```

Most common use
    Efficiently desrialize a large, complicated JavaScript data structures
    reveived over network via XMLHttpRequest
What does it cost to have eval in the language?
    Can you do this in C? What would it take to implement?

<br>

### Unusal features of JavaScript

Some built-in functions
    eval, runtime type checking, etc.
Regular expressions for string pattern matching<br>
Add, delete methods of a object dynamically

<br>

## 14.1 Objects of JavaScript

### 14.1.1 Encapsulation

Encapsulation syntactically separates interface implementation.

- Interface : public
- Implementation : private

<br>

### 14.1.2 Methods

Methods are properties holding fuinction values.

```js
let rabbit = {};
rabbit.speak = function (line) {
    console.log("The rabbit says '${line}'");
}
rabbit.speak("I'm alive.");
// The rabbit says 'I'm alive.'
```

The variable <b>this</b>

```js
function speak(line) {
    console.log("The ${this.type} rabbit says '${line}'");
}
let whiteRabbit = { type : "white", speak };
let hungryRabbit = { type : "hungry", speak };
whiteRabbit.speak("Oh my ears and whiskers, how late it's getting!");
// The white rabbit says 'Oh my ears and whiskers, how its's getting!'
hungryRabbit.speak("I could use a carrot right now.");
// The hungry rabbit says 'I could use a carrot right now.'
```

Explicit passing of the argument <b>this</b> : let it be the first one

```js
speak.call(hungryRabbit, "Burp!");
// The hungry rabbit says 'Burp!'
```

- "this" and arrow functions : dynamic binding
- Anonumous function with the keyword function may not work.

```js
function normalize() {
    console.log(this.coords.map(n => n / this.length));
}
normalize.call({coords : [0, 2, 3], length : 5 });
// [0, 0.4, 0.6]
```

<br>

### 14.1.3 Prototypes

- The object of the base class or superclass.
- The root object is Object.prototype.

```js
console.log(Object.getPrototypeOf({}) == Object.prototype);
// true
console.log(Object.getPrototypeOf(Object.prototype));
// null
console.log(Object.getPrototypeOf(Math.max) == Function.prototype);
// true
console.log(Object.getPrototypeOf([]) == Array.prototype);
// true
```

- Object.create to create an object with a specific prototype

```js
let protoRabbit = {
    speak(line) {
        console.log("The ${this.type} rabbit says '${line}'");
    }
};

let killerRabbit = Object.create(protoRabbit);
killerRabbit.type = "killer";
killerRabbit.speak("SKREEEE!");
// The killer rabbit says 'SKREEEE!'
```

<br>

### 14.1.4 Classes

- An object is an instance of a class.
- An object can be defined by an constructor.
- Any function can be a constructor.

```js
function makeRabbit(type) {
    let rabbit = Object.create(protoRabbit);
    rabbit.type = type;
    return rabbit;
}
```

- Prototype object is stored in the prototype property.
- Keywork <b>new</b> treats the following function a constructor.

```js
function Rabbit(type) {
    this.type = type;
}
Rabbit.prototype.speak = function(line) {
    console.log("The ${this.type} rabbit says '${line}'");
}
let weirdRabbit = new Rabbit("weird");
```

<br>

### 14.1.5 Class notation

- JavaScript classes are constructor functions with prototype property.

```js
class Rabbit {
    constructor(type) {
        this.type = type;
    }
    speak(line) {
        console.log("The ${this.type} rabbit says '${line}'");
    }
}
let killerRabbit = new Rabbit("killer");
let blackRabbit = new Rabbit("black");
```

- Class definition allows only method properties.
- Unnamed class definition

```js
let object = new class { getWord() { return "hello"; }};
console.log(object.getWord());
// hello
```

<br>

### 14.1.6 Overriding derived properties

- Similar to the mechanism of virtual functions in C++.

```js
Rabbit.prototype.teeth = "small";
console.log(KillerRabbit.teeth);
// small

killerRabbit.teeth = "long, sharp, and bloody";
console.log(killerRabbit.teeth);
// long, sharp, and bloody

console.log(blackRabbit.teeth);
// small

console.log(Rabbit.prototype.teeth);
// small

console.log(Array.prototype.toString == Object.prototype.toString);
// false

console.log([1, 2].toString());
// "1, 2"

console.log(Object.prototype.toString.call([1, 2]));
// [object Array]
```

<br>

### 14.1.7 Maps

- An object can serve as a mapping or table.
- But an object includes many unneccessary properties as a table because of being an "object".
- Making a pure table : create an object with null prototype, Object.create(null)
- By using the class Map

```js
let ages = new Map();
ages.set("Boris", 39);
ages.set("Liang", 22);
ages.set("Jlia", 62);

console.log("Jlia is ${ages.get("Jlia")}");
// Jlia is 62

console.log("Is Jack's age known?", ages.has("Jack"));
// Is Jack's age known? false

console.log(ages.has("toString"));
// false
```

- get, set and has are inherent properties of the class Map.
- The method hasOwnProperty is useful when treating an object as a table.


```js
console.log({x : 1}.hasOwnProperty("x"));
// true

console.log({x : 1}.hasOwnProperty("toString"));
// false
```

<br>

### 14.1.8 Polymorphism

- Property overriding provides us with polymorphism

```js
Rabbit.prototype.toString = function () {
    return "a ${this.type} rabbit";
}

console.log(String(blackRabbit));
// a black rabbit
```

<br>

### 14.1.9 Symbols

- Symbols are values created with the Symbol function.
- A symbol is unique.

```js
let sym = Symbol("name");
console.log(sym == Symbol("name"));
// false

Rabbit.prototype[sym] = 55;
console.log(blackRabbit[sym]);
// 55
```

- A symbol can be a property name.

```js
const toStringSymbol = Symbol("toString");
Array.prototype[toStringSymbol] = function () {
    return "${this.length} cm of blue yarn";
}

console.log([1, 2].toString());
// "1, 2"

console.log([1, 2][toStringSymbol]());
// 2 cm of blue yarn
```

- It is possible to include symbol properties in object expressions and classes by using square brackets around the property name.

```js
let stringObject = {
    [toStringSymbol]() { return "a jute rope"; }
};

console.log(stringObject[toStringSymbol]());
// a jute rope
```

<br>

### 14.1.10 The iterator interface

- An iterable object has a method named with the symbol <b>Symbol.iterator</b>.
- next, value, and done property names are plain strings, not symbols.

```js
let okIterator = "OK"[symbol.iterator]();

console.log(okIterator.next());
// {value: "O", done: false}
console.log(okIterator.next());
// {value: "K", done: flase}
console.log(okIterator.next());
// {value: undefined, done: true}
```

- Matrix example

```js
class Matrix {
    constructor(width, height, element = (x, y) => undefined) {
        this.width = width;
        this.height = height;
        this.content = [];

        for (let y = 0; y < height; y++) {
            for (let x = 0; x < width; x++) {
                this.context[y * width + x]  = element(x, y);
            }
        }
    }
    get(x, y) {
        return this.content[y * this.width + x];
    }
    set(x, y, value) {
        this.content[y * this.width + x] = value;
    }
}

class MatrixIterator {
    constructor(matrix) {
        this.x = 0;
        this.y = 0;
        this.matrix = matrix;
    }
    next() {
        if (this.y == this.matrix.height) return {done: true};
        let value = {x : this.x, y : this.y, value : this.matrix.get(this.x, this.y)};
        
        this.x++;
        if (this.x == this.matrix.width) {
            this.x = 0;
            this.y++;
        }

        return {value, done: false};
    }
}
```

- Setting up a matrix with an iterator

```js
Matrix.prototype[Symbol.iterator] = function() {
    return new MatrixIterator(this)
};
```

- looping over the matrix

```js
let matrix = new Matrix(2, 2, (x, y) => 'value ${x}, ${y}');
for (let {x, y, value} of matrix) {
    console.log(x, y, value);
}
// 0 0 value 0, 0
// 1 0 value 1, 0
// 0 1 value 0, 1
// 1 1 value 1, 1
```
<br>

### 14.1.11 Getters, setters, and statics

- Properties hiding method calls.
- Methods belonging to a class, not objects of the class : <b>static</b>
- Attached to the front of the constructor.

```js
class Temperature {
    constructor(celsius) {
        this.celsius = celsius;
    }
    get fahrenheit() {
        return this.celsius * 1.8 + 32;
    }
    set fahrenheit(value) {
        this.celsius = (value - 32) / 1.8;
    }
    static fromFahrenheit(value) {
        return new Temperature((value - 32) / 1.8);
    }
}

let temp = new Temperature(22);
console.log(temp.fahrenheit);
// 71.6

temp.fahrenheit = 86;
coneole.log(temp.celsius);
// 30
```

<br>

### 14.1.12 Inheritance

- Inheritance by the prototype system
- Keyword <b>extends</b>
- Superclass-subclass relationship
- Benefits
    - Code reuse
    - Subtype polymorphism

```js
class SymmetricMatrix extends Matrix {
    constructor(size, element = (x, y) => undefined) {
        super(size, element (x, y) => { // calling the constructor of Matrix
            if(x < y) return element(y x);
            else return element(x, y);
        });
    }
    set(x, y, value) {
        super.set(x, y, value); // calling the set method of Matrix
        if(x != y) {
            super.set(y, x, value);
        }
    }
}

let matrix = new SymmetricMatrix(5, (x, y) => '${x}, ${y}');
console.log(matrix.get(2, 3));
// 3, 2
```

<br>

### 14.1.13 InstanceOf operator

```js
console.log(new SymmetricMatrix(2) instanceof SymmetriocMatrix);
// true

console.log(new SymmetrixMatrix(2) instanceof Matrix);
// true

console.log(new Matrix(2, 2) instanceof SymmetricMatrix);
// false

console.log([1] instanceof Array);
// true
```