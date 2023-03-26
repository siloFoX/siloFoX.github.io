---
layout: post
title:  "Programming language final exam study 1"
date:   2020-06-20
excerpt: "Control"
tag:
- PL
- programming language
- 프로그래밍 언어
- blog
- silofox
comments: true
lastmod : 2020-06-20
sitemap : 
  changefreq : never
  priority : 1.0
feature: https://github.com/siloFoX/siloFoX.github.io/blob/master/images/programming-language/programming-language-feature.jpg?raw=true
---

## 9.2 Exception 

### 9.2.1 Purpose of Exception Mechanism

- jump out of a block or function invocation
- pass data as part of the jump
- return to a program point that was set up to continue the computation

>
예외(Exception)에 대해서 그리고 예외의 목적
>
- 바깥블록으로 탈출하기 위해서, 혹은 함수 호출(invacation 혹은 call)을 위해
- 데이터의 일부를 jump 의 일부로 보내기 위해(jump 할 때 데이터를 가지고 가고 싶은 것)
- 계속 계산을 하고 싶은 부분으로(프로그램에서) 다시 이동하고자 할 때
>
아마 일반적인 우리가 생각하는 Exception 의 용법(에러에 준하는 논리적인 문제가 생겼을 때 처리)<br>
보다는 프로그래밍 언어적인 jump 에 집중한 것 같다.<br>
당연히 우리가 예외처리하는 것은 high-level 입장이니까.
>

<br>

#### Exception mechanism

- throwing : statement or expression form for raising exception
- catching : handler. If there are more than one handler, dynamic scoping is used for resolution

>
예외처리 메커니즘
>
- throwing : 예외를 던지는 표현식 또는 문장(물론 프로그램 관점의.)
- catching : 핸들러(event-driven 방식의.). 만약 여러개의 핸들러가 있다면 동적 범위가 적용된다.(우선 적용 순서가 있다는 것.)
>

<br>

### 9.2.2 JavaScript Exception 

- The try-catch syntax
```js
try {
    // some code
} catch (err) {
    // error handler
}
// execution continues here
```
- <b>error</b> contains error object. Sometimes omitted. (Details in below)
- Try-catch works for only runtime errors.
- Exception handlers do not work for syntax errors.

>
우리에게 익숙한 try-catch 구문
>
- 'error' 라는 이름의 객체를 생성한다. 그리고 에러와도 가끔 무시한다.
- Try-catch 는 런타임 에러들만 담당한다. (논리적으로 에러라고 하는 것들만)
- Exception handler 들은 구문 오류를 잡지는 않는다.
>
아주 당연한 얘기들 밑에 있는 내용이 더 중요하다.
>

- Try-catch work synchronously.
```js
try {
    setTimeout(function() {
        noSuchVariable; // script will die here
    }, 1000);
} catch (e) {
    // won't work
}
```
- Compare with
```js
setTimeout(function() {
    try {
        noSuchVariable; // try..catch handles the error!
    } catch {
        console.log("error is caught here!");
    }
}, 1000);
```
- An error object has two standard properties
    - name
    - message

    Sometimes
    - stack : runtime stack

#### Example 9.1 An error object has the properties of name, message, and stack.

```js
try {
    lalala; // error, variable is not defined!
} catch (err) {
    console.log(err.name); // ReferenceError
    console.log(err.message); // lalala is not defined
    console.log(err.stack); // ReferenceError: lalala is not defined

    // Can also show an error as a whole
    // The error is converted to string as "name: message"
    console.log(err); // ReferenceError: lalala is not defined
}
```

- Bulit-in constructors for standard errors : Error, SyntaxError, ReferenceError, TypeError
```js
let error = new Error(message);
// or
let error = new SyntaxError(message);
let error = new ReferenceError(message);
// ...
```

- Throwing an error<br>
<b>Example 9.2 JSON.parse detects an error and thorows an exception.</b>
```js
let json = "{ bad json }"; // '{ "name" : "John", "age" : 30 } is fine
try {
    let user = JSON.parse(json); // <-- when an error occurs...
    console.log(user.name); // doesn't work
} catch (e) {
    // ...the execution jumps here
    console.log("Our apologies, the data has errors, we'll try to request it one more time.");
    console.log(e.name);
    console.log(e.message);
}
```
Programmers can force activation of an exception handler.
```js 
let json = '{ "age" : 30 }'; // incomplete data
try {
    let user = JSON.parse(json); // <-- no errors
    if (!user.name) {
        throw new SyntaxError("Incomplete data: no name"); // (*)
    }
    console.log(user.name);
} catch (e) {
    console.log("JSON Error : " + e.message);
    // JSON Error : Incomplete data: no name
}
```

- Error names are the constructor names.

    ```js
    let error = new Error("Things happen o_O");

    console.log(error.name); // Error
    console.log(error.message); // Things happen o_O
    ```

- Rethrowing : raising an exception again when cannot be handled
```js
let json = '{ "age" : 30 }'; // incomplete data
try {
    let user = JSON.parse(json);
    if (!user.name) {
        throw new SyntaxError("Incomplete data: no name");
    }
    blabla(); // unexpected error
    console.log(user.name);
} catch (e) {
    if (e.name == "SyntaxError") {
        console.log("JSON Error: " + e.message);
    } else {
        throw e; // rethrow (*)
    }
}
```

- try-catch-finally
```js
try {
    console.log('try');
    if (confirm('Make an error?')) BAD_CODE();
} catch (e) {
    console.log('catch')
} finally {
    console.log('finally');
}
```

- Data passing between a throw and catch

    ```js 
    function readData() {
        let json = '{ "age" : 30 }';
        try {
            // ...
            blabla(); // error!
        } catch (e) {
            // ...
            if (e.name != 'SyntaxError') {
                throw e; // rethrow (don't know how to deal with it)
            }
        }
    }

    try {
        readData();
    } catch (e) {
        console.log("External catch got: " + e); // caught it!
    }
    ```

#### Example 9.3

```js 
console.log("Enter a positive integer number?", 35)

let num = readline();
let diff, result;

function fib(n) {
    if (n < 0 || Math.trunc(n) != n) {
        throw new Error("Must not be negative, and also an integer.");
    }
    return n <= 1 ? n : fib(n-1) + fib(n-2);
}

let start = Dat.now();
try {
    result = fib(num);
} catch (e) {
    result = 0;
} finally {
    diff = Date.now() - start;
}
console.log(result || "error occured");
console.log('excution took ${diff}ms');
```

- finally and return 
```js 
function func() {
    try {
        return 1;
    } catch (e) {
        /* ... */
    } finally {
        console.log('finally');
    }
}
console.log(func()); // first works output from finally, and then this one
```

- try-finally
```js
function func() {
    // start doing something that needs completion (like measurements)
    try {
        // ...
    } finally {
        // complete that thing even if all dies
    }
}
```

#### Example 9.4 
```js
let e = new Error();
function f(x) {
    if (x == 0) e.v = 0;
    else if (x == 1) e.v = 1;
    else if (x == 10) e.v = x - 8;
    else return (x - 2) % 4;
    throw e;
    console.log("end of function f");
}

let y = 7;
try {
    y = f(1);
} catch (err) {
    console.log(err.v);
}
```

<br>

### 9.2.3 C++ Exception

try-catch-throw.

- try
```c++
try {
    // statements that may throw exceptions
}
```

- throw 
```c++
throw "this generates a char * exception";
```

- catch
```c++
catch (char *message) {
    // statements that process the thrown char * exception
}
```

- Multiple catch
```c++
try {
    // code may throw char pointers and other pointers
}
catch (char *message) {
    // code processing the char pointers thrown as exceptions
}
catch (char *whatever) {
    // code processing all other pointers thrown as exceptions
}
```

- Variations in triggering exception handler : Difference between ML exception and C++ exception
    - Pattern matching vs. type matching
    - Garbage collected vs. program managed

- Problematic type matching
```c++
catch(T t)
catch(const T t)
catch(T& t)
catch(const T% t)
```
These can catch exception objects of type E if
    - T and E or the same type, or
    - T is an accessible base class of E at the throw point, or
    - T and E are pointer types and E can be converted to T a the throw time

<br>

### 9.2.4 More about Conditions

<b>Exceptions for Error Conditions</b>

```
data type 'a tree = Leaf of 'a | Node of 'a tree * 'a tree;
exception No_Subtree;

fun lsub(Leaf x) = raise No_Subtree
|   lsub(Node(x,y)) = x;
```

<b>Exceptions for Efficiency</b>
 
Compare the following two codes.

```js
function prod(x) {
    if (typeof x == 'number') return x;
    else return prod(x.left) * prod(x.right);
}

function ex_prod(aTree) {
    let zero = new Error("zero");
    function p(x) {
        if (x instanceof "Number") {
            if (x == 0) throw zero; else return x;
        }
        else return p(x.right) * p(y.left);
    }
    try { return p(aTree); } catch (err) { return 0; }
}
```

<b>Static and Dynamic Scope</b> 

Static scope for expressions and dynamic scope for exceptions.

```js
let x = 6;
function f(y) { return x; }
function g(h) {
    let x = 2;
    return h(1);
}
{
    let x = 4;
    console.log(g(f));
}
```

Simulation dynamic scope using exceptions

```js
let x = new Error();
try {
    function f(y) { throw x; }
    function g(h) {
        try { h(1); } catch (err) { x.v = 4; }
    }
    try { g(f); } catch (err) { x.v = 4; }
} catch (err) { x.v = 6; }
console.log(x.v);
```

<b>Typing and Exceptions</b> 

Some languages require both normal exceptional flows coincide with each other in type.<br>
For example, e1 and e2 should have the same type in ML.

```
e1 handle A => e2;
```
e1 and e2 should have type int.
```
1 + (e1 handle A => e2);
```

<b>Exceptions and Resource Allocation</b>

- Activation records under the handlers' will be reclaimed after an exception is raised.
- Garbage collection needed.
- Without garbage collection
    - Automatic invoking of destructors as in C++
    - Live with memory leak.

<br>

## Continuation

- Similar to callbacks or upcalls
- Continuation-passing-style
- Guy Steele and Gerald Sussman

<br>

### 9.3.1 A Function Representing "The Rest of the Program"

- What is the continuation of 1.0/y?
```js
function f(x, y) { return 2.0*x + 3.0*y + 1.0/x + 2.0/y; }
```
Another version
```js
function f(x, y) {
    let before = 2.0*x + 3.0*y;
    function conti(q) { return before + q + 2.0/y; };
    conti(1.0/x);
}
```

- Normal continuation and error continuation

    ```js
    function divide (n, d, normal_cont, error_cont) {
        if(d > 0.0001) 
            return normal_cont(n/d);
        else
            return error_cont();
    }

    function f(x, y) {
        let before = 2.0*x + 3.0*y;
        function conti(q) { return before + q + 2.0/y; }
        function error_cont() { return before / 5.2; }
        return divide(1.0, x, conti, error_conti);
    }
    ```
    Using exception
    ```js
    function f(x, y) {
        try {
            return (2.0*x + 3.0*y + 1.0/(x > 0.0001 ? x : throw new Error("Div by zero")));
        } catch (err) { return (2.0*x + 3.0*y) / 5.2; }
    }
    ```

<br>

### 9.3.2 Continuation-Passing Form and Tail Recursion

- Passing continuation is passed to a function.
- The function does not have to return to the caller.
- Similar to tail call.
- Transformation of factorial into CPS.
    ```js 
    function fact(n) { if (n == 0) return 1 else return n*fact(n-1); }
    ```
    - fact(9) calls fact(8)
    - continuation of fact(8) is λx.9 * x
    - fact(8) calls fact(7)
    - continuation of fact(7) is λy.(λx.9 * x)(8 * y)
    - In general, the continuation of fact(n-1) is the composition of λx.n*x with the continuation of fact(n).
    ```
    f(n, k) = if n=0 then k(1) else f(n-1, λx.k(n*x))
    ```
    Expanding f(3, λx.x)
    ```
    f(3, λx.x) = f(2, λy.((λx.x)(3*y)))
               = f(1, λx.((λy.3*y)(2*x)))1
               = 6
    ```
- Continuation and tail recursion
```js
function fact(n) {
    function f(n, k) {
        if (n == 0) return k(1) else return f(n-1, x => k(n*x))
    }
    return f(n, (x) => x);
}
```
- Consider space requirement, and compare with the following
```js
function fact(n) {
    function f(n, k) {
        if (n == 0) return k else return f(n-1, n*k)
    };
    return f(n, 1);
}
```

<br>

## 9.4 Functions and Evaluation Order

- Example
```js
function f(x, y) { ...x...y... }
f(e1, e2);
```

- the value of y is necessary only if the value of x meets some condition 
- the evalution of e2 is expensive
```js
function fun(x, y) { ...x...Force(y)... }
f(e1, Delay(e2));
```

- Two topics :
    - Expressing Delay and Force in conventional programming languages
    
    ```js
    function Delay(e) { return () => e; }
    function Force(e) { e(); }

    function time_consuming(n) {
        function tak(x, y, z) {
            if (x <= y) return y;
            else return tak(tak(x-1, y, z), tak(y-1, z, x), tak(z-1, x, y));
        }
        tak(3*n, 2*n, n)
    }

    function fib (n) {
        if (n == 0 || n == 1) return 1;
        else return fib(n-1) + fib(n-2);
    }
    function odd(n) { return (n mod 2) == 1; }
    function f(x, y) { if odd(x) return 1 else fib(y()); }
    f(fib(9), time_consuming(9));

    function lazy_f (x, y) { if odd(x) return 1 else fib(y()); }
    lazy_f(fib(9), () => time_consuming(9));
    ```

- If a delyed values is needed more than once... : transmission-by-need, put flag using properties

    ```js
    function Delay (e) { return { expr : () => e, val : undefined }; }
    function force (d) {
        let v = ev(d);
        if(d.val == undefined) { d.val = v; }
        return v;
    }
    function ev ({expr : e, val : s }) {
        if (s == undefined) return e(); else return s;
    }

    let d = Delay(time_consuming(9));
    force(d);
    ```