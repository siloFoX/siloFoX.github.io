---
layout: post
title:  "Programming language mid-term exam study - types"
date:   2020-05-10
excerpt: "프로그래밍 언어 중간고사 준비 - 타입에 관하여"
tag:
- PL
- programming language
- 프로그래밍 언어
- types
- mid-term
- 중간고사
- blog
- silofox
comments: true
---

일단 pdf 에 있는 것들 해석하면서 모르는 내용 채워가자.

## Chapter 7 Types

### 7.1 Types in programming

A type is a collection of computatinal entities, for example int.
- naming and organizing concpet
- consistent interpretation of bit sequence in memory
- providing compilers with information about data

>
타입이란 계산이 가능한 개체들의 집합이다. 예를 들어서 정수형.
- 이름 짓는 것과 개념 구성하기 (어떤 데이터를 담을 것인지)
- 메모리안에 bit 들의 순서를 일관성있게 해석하기 (int 면 4byte 이런거?)
- 데이터에 대한 정보를 컴파일러에 전달하기
>

#### 7.1.1 Program Organization and Documentation 

Design types reflecting the real world from which a problem arises.

>
문제가 일어나는 실제 세상을 반영하는 타입을 디자인해라. (그냥 팁 같은건가;;)
>

#### 7.1.2 Type errors

- Hardware errors
- Unintended semantics

>
- 하드웨어 에러들
- 의도치 않은 의미들 (semantic error를 말한다.)
>

#### 7.1.3 Types and optimization

Using compile-time type information to generate efficient machine instructions.

>
효율적인 명령어를 생성하기 위해서 Compile-time 에서 type 정보를 활용한다.
>

### 7.2 Type safety and type checking

- A programming language is type safe if no program is allowed to voilate its type distinictions.
- C is not type safe because of type casts, pointer arithemetic, and explicit deallocation and dangling pointers.
  - An integer can be cast to a function.
  - Pointer x = *(p + i) is not type safe.
  - Dangling pointer makes a program not type safe.
- Java and Lisp are type safe.

>
- 프로그램이 타입의 구분을 침범하지 않을 때 그 프로그래밍 언어는 type safe 라고 한다.
- C 는 type safe 가 아니다. 왜냐하면 타입 캐스트들, 포인터 연산, 명시적인 할당 해제, 그리고 dangling 포인터들 때문이다. 
  - 정수형 타입은 함수로 형변환이 가능하다.
  - x = *(p + i) 형태의 포인터는 type safe 하지 않다.
  - Dangling pointer 는 프로그램을 not type safe 하게 만든다.
- Java 와 Lisp 은 type safe 하다.
>

type safe 에 대한 추가적인 조사를 하자.

#### 7.2.1 Compile-time and Run-time checking

- Runtime type checking : slow down program execution
- Compile-time type checking : sound and conservative
- Combined : Java's array bound checking

>
- 런타임에 타입 체크 : 프로그램이 느리다.
- 컴파일 시간에 타입 체크 : 건전하고 보수적이다.
- 결합된 형태 : 자바에서 행렬 범위 체크
>

type checking 에 대한 추가적인 조사

### 7.3 Type inference

- Type checking vs. type inference
- Polumorphism

>
- 타입 체킹 vs. 타입 추론
- 동형성
>

추론과의 차이점에 대한 조사

동형성에 대해 자세한 정보 조사

#### Type-inference algorithm

1. Assignment of types to expressions and subexpressions.
2. Generation of type constraints.<br>
    Function application If f : τ1, e : τ2, and fe : τ3 then τ1 = τ2 -> τ3<br>
    Lambda Abstraction (Function Expression) If x : τ1 and e : τ2 then λx.e : τ1 -> τ2.
3. Solving the constraints using unification.

>
1. 표현식과 하위표현식에 대한 타입들의 할당.
2. 타입 제한들의 생성.
3. Unification 을 써서 제한을 푼다.
>

예시들을 보면서 차근차근 익힌다.

### 7.4 Polymorphism and Overloading

- Parametric polymorphism
  - Explixit : C++
    ```c++
    template <typename T>
    void swap (T &x, T& y) {

        T tmp = x;
        x = y;
        y = tmp;
    }
    ```
  - Implicit : Lisp, ML
- ad hoc polymorphism (Overloading).
- subtype polymorphism : Object-oriented programming
    ```java
    class B extends C { ... }
    B x, a;
    C y, b;

    y = x; // well-typed
    a = b; // ill-typed
    ```

>
- 매개변수적인 동형성
  - 명시적인 언어 : C++
  - 암시적인 언어 : Lisp, ML
- 에드 혹 동형성 (오버로딩)
- 하위 타입 동형성 : 객체지향 프로그래밍
>

동형성의 분류에 대한 자세한 조사