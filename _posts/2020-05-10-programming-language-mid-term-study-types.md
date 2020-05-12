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
feature: https://github.com/siloFoX/siloFoX.github.io/blob/master/images/programming-language/programming-language-feature.jpg?raw=true
---

<br>
일단 pdf 에 있는 것들 해석하면서 모르는 내용 채워가자. <br>
포스트 형식 :
- 원문
- 해석
- 부가설명

<br><br>

## Chapter 7 Types

### 7.1 Types in programming

A type is a collection of computatinal entities, for example int.
- naming and organizing concpet
- consistent interpretation of bit sequence in memory
- providing compilers with information about data

<br>

>
타입이란 계산이 가능한 개체들의 집합이다. 예를 들어서 정수형.
- 이름 짓는 것과 개념 구성하기 (어떤 데이터를 담을 것인지)
- 메모리안에 bit 들의 순서를 일관성있게 해석하기 (int 면 4byte 이런거?)
- 데이터에 대한 정보를 컴파일러에 전달하기
>

<br>

    computational entites : 그 자체로 의미를 생성하는 집합 (자료구조, 함수 ...)

    개념을 구성하면 프로그램하기가 편해진다.

    데이터에 대한 정보를 metadata 라고 한다.

<br><br><br>

#### 7.1.1 Program Organization and Documentation 

Design types reflecting the real world from which a problem arises.

<br>

>
문제가 일어나는 실제 세상을 반영하는 타입을 디자인해라. (그냥 팁 같은건가;;)
>

<br><br><br>

#### 7.1.2 Type errors

- Hardware errors
- Unintended semantics

<br>

>
- 하드웨어 에러들
- 의도치 않은 의미들 (semantic error를 말한다.)
>

<br><br><br>

#### 7.1.3 Types and optimization

Using compile-time type information to generate efficient machine instructions.

<br>

>
효율적인 명령어를 생성하기 위해서 Compile-time 에서 type 정보를 활용한다.
>

<br><br><br>

### 7.2 Type safety and type checking

- A programming language is type safe if no program is allowed to voilate its type distinictions.
- C is not type safe because of type casts, pointer arithemetic, and explicit deallocation and dangling pointers.
  - An integer can be cast to a function.
  - Pointer x = *(p + i) is not type safe.
  - Dangling pointer makes a program not type safe.
- Java and Lisp are type safe.

<br>

>
- 프로그램이 타입의 구분을 침범하지 않을 때 그 프로그래밍 언어는 type safe 라고 한다.
- C 는 type safe 가 아니다. 왜냐하면 타입 캐스트들, 포인터 연산, 명시적인 할당 해제, 그리고 dangling 포인터들 때문이다. 
  - 정수형 타입은 함수로 형변환이 가능하다.
  - x = *(p + i) 형태의 포인터는 type safe 하지 않다.
  - Dangling pointer 는 프로그램을 not type safe 하게 만든다.
- Java 와 Lisp 은 type safe 하다.
>

<br>

    type safe 에 대한 추가적인 조사를 하자.

    (중요) type safe : type check 이후에 문제가 없음을 보장하는 것
    C 는 런타임에서 type 때문에 에러가 발생 가능하다.

    dangling pointer : free 했지만 포인터 변수는 남아있기 때문에 접근 가능한 것

    - 정수형 타입 : int -> int * -> function * 로 변환 가능
    - 포인터 연산 자체가 보안상 위험하다

    Java : 강력한 타입 선언, 컴파일 체크, 런타임 체크
    Lisp : 인터프리터가 강력하게 체크한다.

<br><br><br>

#### 7.2.1 Compile-time and Run-time checking

- Runtime type checking : slow down program execution
- Compile-time type checking : sound and conservative
- Combined : Java's array bound checking

<br>

>
- 런타임에 타입 체크 : 프로그램이 느리다.
- 컴파일 시간에 타입 체크 : 건전하고 보수적이다.
- 결합된 형태 : 자바에서 행렬 범위 체크
>

<br>

    type checking 에 대한 추가적인 조사가 필요하다.

    sound and conservative : correct 하지만 overkill && 보수적인 접근
    (괜찮을 수도 있는데 경고 한다.)

<br><br><br>

### 7.3 Type inference

- Type checking vs. type inference
- Polumorphism

<br>

>
- 타입 체킹 vs. 타입 추론
- 동형성
>

<br>

    추론과의 차이점에 대한 조사필요
    동형성에 대해 자세한 정보 조사필요

    type checking : expression 을 이루는 type 이 소스코드와 문제가 없는가
    type inference : 코드에 나타나지 않는 type 을 조사하여 추론함. (명시적인 type 언어가 아닌거지)

<br><br><br>

#### Type-inference algorithm

1. Assignment of types to expressions and subexpressions.
2. Generation of type constraints.<br>
    Function application If f : τ1, e : τ2, and fe : τ3 then τ1 = τ2 -> τ3<br>
    Lambda Abstraction (Function Expression) If x : τ1 and e : τ2 then λx.e : τ1 -> τ2.
3. Solving the constraints using unification.

<br>

>
1. 표현식과 하위표현식에 대한 타입들의 할당.
2. 타입 제한들의 생성.
3. Unification 을 써서 제한을 푼다.
>

<br>

    함수 타입 : 인자 타입(parameter) -> 결과 타입(return)
    람다 함수의 타입 = bound -> expression

    unification : unifier 를 찾는 행위
    unifier : 제약조건이 가장 일반적인 sub-expression (간단한 식 형태를 말하는건가)

    예시들을 보면서 차근차근 익힌다.
    (람다에 대한 선행공부가 필요하다.)

<br><br><br>

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
    class B extends C { ... } // B는 C의 sub-type 이라고 생각할 수 있다.
    B x, a; // 자식 (sub-type)
    C y, b; // 부모

    y = x; // well-typed (부모에서 자식을 참조하는 건 ok)
    a = b; // ill-typed (자식에서 부모을 참조하는 건 x)
    ```

<br>

>
- 매개변수적인 동형성
  - 명시적인 언어 : C++
  - 암시적인 언어 : Lisp, ML
- 에드 혹 동형성 (오버로딩)
- 하위 타입 동형성 : 객체지향 프로그래밍
>

<br>

    동형성의 분류에 대한 자세한 조사가 필요하다.

    Explicit : 컴파일 시점에 각각 타입이 정의된 함수를 전부 생성한다.
    Implicit : 런타임 때 type 을 정의한다. 딱 하나의 카피만을 가진다. 
    (함수 한개만 생성하고 타입에 따라 다르게 실행한다는 뜻인가보다)

    ad hoc : 이유 없이 프로그래머가 결정한 함수(중복된 이름을 가진 다른 함수)

    (헷갈릴 만한 것)
    overloading : 같은 이름의 메소드
    overrriding : 상위 클래스의 메소드를 재정의