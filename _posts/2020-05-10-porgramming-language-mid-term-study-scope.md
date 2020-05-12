---
layout: post
title:  "Programming language mid-term exam study - scope"
date:   2020-05-10
excerpt: "프로그래밍 언어 중간고사 준비 - 범위에 관하여"
tag:
- PL
- programming language
- 프로그래밍 언어
- scope
- mid-term
- 중간고사
- blog
- silofox
comments: true
feature: https://github.com/siloFoX/siloFoX.github.io/blob/master/images/programming-language/programming-language-feature.jpg?raw=true
---

## Chapter 8 Scope, Functions, and Storage management

### 8.1 Block-structured Languages

#### Block 

- Explicit begin and end (other variants)
- May introduce new names (local declarations)
- A name is either local or global. Data or functions.
- When reffering a global variable, the name declared in the closest enclosing block is selected.
  ```js
  { // outer block
    let x = 2
    {   // inner block // 여기서 activation record 생산(stack)
        let y = 2;
        x + y + 2; // 그런데 x 값에 대한 갱신이 없다.
    }   // record 삭제 
  }
  ```

<br>

>
#### 블록
- 명시적인 시작과 끝 (다른 변형들)
- 아마도 새로운 이름을 쓸 것이다. (지역 선언들)
- 이름의 범위가 지역이든 전역이든, 데이터혹은 함수이다.
- 전역변수를 참조할 때(쓰려고 할 때), 가장 가까운블록에 둘러쌓여있는 이름이 선택된다.
>

<br>

    블록 : 명확한 시작과 끝이 있어야 하고, "이름"이 등장하는 단위여야 한다.
    
    C 계열에서는 각 block 마다 stack frame 을 두지 않는다.
        -> 함수 블록 단위로만 만든다.
        -> stack pointer 가 움직이지 않는다는 뜻

<br><br><br>

- Nested or independent. No partial overlapping
- Block structure allows implementation of recursion.
- When instructions of a block are executed, memory for the locals is allocated.
- When exiting a block, the allocated memory may be deallocated.
- Classification of variables
  - Local variables
  - Parameters
  - Global variables
- Simple memory model : Register + Text + Data (Stack and Heap)

<br>

>
- 중첩되든 독립적이든. 부분적인 중첩은 없다. (부분적으로 범위가 작동되지 않는다는 뜻인 것 같다.)
- 블록 구조는 재귀의 적용을 허락한다.
- 블록에서 명령이 실행되면, 지역적인 메모리가 할당된다.(그 블록 안에서 쓰는?)
- 블록이 닫히면, 할당되었던 메모리를 풀어준다.
- 변수들의 분류
  - 지역 변수들
  - 매개변수들
  - 전역 변수들
- 간단한 메모리 모델(구조) : 레지스터 + 문자 + 데이터 (스택과 힙)
>

<br>

    블록의 입장에서 local variable 과 parameter 는 같다.

<br><br><br>

### 8.2 In-line Blocks
(if, for, ...)

#### 8.2.1 Activation Records and Local Variables

Stack frame and allocation of local variables<br>
Intermediate results - temporary variables
```js
{ 
    let x = 0;
    let y = x + 1;
    {
        let z = (x + y) * (x - y);
    }
}
```

<br>

>
스택 프레임과 지역변수들의 할당
중간 결과들 - 일시적 변수들 ( 중괄호 scope 안에 있는 것들은 일시적으로 할당되었다가 풀리는 것을 말하는 듯.)
>

<br>

    Control link 는 상위 블록을 가리킨다.
    
    Activation Records : Control link, local variables, Intermediate results
        -> Intermediate results : 실제로는 레지스터에 저장된다.

<br><br><br>

Scope and Lifetime<br>
Scope : a region of text in which a declaration is visible<br>
Lifetime : the duration, during a run of a program, during which a location is allocated as the result of a specific declaration.
```js
{
    let x = ...;
    {
        let y  = ...;
        {
            let x = ...; // holes in the scope of 
                         // the outer declartion of x
            ...          // (밖에 있는 x 의 scope 를 무시한다는 뜻)
        }
    }
}
```
<br>

>
범위와 수명
범위 : 선언이 보이는 텍스트 영역 (선언의 영향이 미치는 영역)
수명 : 프로그램이 돌아가는 동안 특정한 선언의 결과로 위치가 할당되는 기간
>

<br>

    Scope : 선언이 유효한 범위 (프로그램 코드의 영역)
        ->  선언 ~ (가려질 수 있는 부분) ~ 블록 끝까지

    Lifetime : Activation 생성 ~ 소멸 까지의 시간

<br><br><br>

### 8.2.2 Global Variables and Control Links

- Control link = dynamic link
- Counting the number of links to follow to access global variables.

<br>

>
- 컨트롤 링크 = 동적인 링크
- 전역변수를 접근하기 위해 따라가야 하는 링크의 숫자를 세는 것.
>

<br>

    Control link : %sp, %fp ... 등으로 구현된 stack 의 link
        -> Activation record 를 역순으로 추적할 수 있도록 만들어짐
        * 그러나 실제로 모든 블록에 개별 record 를 넣지는 않음

<br><br><br>

## 8.3 Functions and Procedures

### 8.3.1 Activation record for function calls

- Control link
- Access link
- Return address
- Return-result address for storing the return value
- Actual parameter
- Local variables
- Temporary

<br>

>
- 컨트롤 링크
- 접속 링크
- 반환 주소
- 반환 값을 저장하기 위한 주소 (반환 값 주소)
- 실제 매개변수
- 지역변수
- 일시적인 것들
>

<br>

    Control link : 호출 순서 (stack 관리)
    
    Access link : 함수 정의로 연결  (Static scoping 을 위해서 필요함)
        * C, Assembly 에서는 Access link 가 필요없다.
          왜냐면 모든 함수가 global 에서 정의되기 때문이다. 
        -> 함수가 함수 정의를 포장하는 Static scoping rule 의 적용을 위해서만 쓰임.

    Actual parameter 는 꼭 레지스터일 필요는 없다.

<br><br><br>

### 8.3.2 Parameter Passing

- formal parameters and actual parameters, how formals are bound to actuals.
- Parameter-passing mehanisms
  - time of evaluation of actual parameters
    - before function body is excuted : side effects, aliasing, efficiency
      - pass-by-refernece (L-value)
      - pass-by-value (R-value)
    - delyed
  - location used to store the parameter value

<br>

>
- 형식적 매개변수와 실제 매개변수, 형식적 매개변수가 어떻게 실제 매개변수와 묶여서 작동하는지 알아보자.
- 매개변수-전달 원리
  - 실제 매개변수의 평가시간
    - 함수가 실행되기 전 : 부작용들, 일그러짐(위신호?), 효율성
      - 참조로 넘어가기 (L-값)
      - 값으로 넘어가기 (R-값)
    - 지연
  - 매개변수값을 저장하는 장소
>

<br>

    형식적 매개변수가 무엇인지 자세하게 설명이 필요하다.

    형식적 매개변수 : 함수의 선언에서 정의된 매개변수이다.
    실제 매개변수 : 실제로 함수에 주는 값

    (중요) Parameter-passing mechanisms
            인자로 들어간 actual parameter 는 언제 계산되는건가?
                * 함수 적용 전에 인자를 평가한다면 : 여러가지 문제들
                    (pass-by-value는 c 스타일로 미리 평가해서 값만 전달한다.)
                * delayed -> reference by name -> expression 에 전달 -> scope 문제 발생
            어디에 매개변수를 저장할 것인가? (일반적으로는 stack)

<br><br><br>

### 8.3.3 Global Variables (First-order case)

#### Finding the declaration of a global identifier

- Static scope : the closest enclosing block of the program text (spatial , text)
- Dynamic scope : most recent activation record (time)

<br>

>
#### 전역 식별자의 선언은 어떻게 되어 있을까
- 정적인 범위 : 프로그램적인 텍스트(공간적이거나, 텍스트적이어야 함)이 둘러쌓인 블록 중에 가장 가까운 것
- 동적인 범위 : 가장 최근 활성화 레코드(시간적인 측면)
>

<br>

    Static scope : 가장 가까운 블록 끝까지가 범위 (text 분석)
    Dynamic scope : Activation record 를 실행된 시간에 역으로 찾는다.

<br><br><br>

#### Access Links for Static Scope

- Static link.
- The access link of an activation record points to the activation record of the closest enclosing block in the program.
- Show example in diagram.

<br>

>
#### 정적인 범위를 위한 접근 링크
- 정적인 링크
- 활성화 레코드의 접근 링크는 프로그램 안의 가장 가까운 둘러쌓인 블록의 활성화 레코드를 가르킨다.
- 그림예시를 확인하자.
>

<br>

     자기가 내표된 가장 가까운 블록을 가리키는 text 에서 블록을 알려줌

<br><br><br>

### 8.3.4 Tail Recursion

- Reuse of activation records.
- Tail call : a call to f in the body of g is a tail call if g returns the result of calling f without any further computation.
- Tail recursive : recursive calls in the body of f that calls f
- Same as iteration

<br>

>
- 활성화 레코드들을 다시 쓴다.
- 꼬리 호출 : 함수 f 를 함수 g 의 안에서 부르는 것이다. 조건으로는 g 에서 반환값으로 f 의 결과를 다른 계산 없이 호출해야한다는 것이다.
- 꼬리 재귀 : f 에서 f 를 재귀적으로 호출하는 것
- 반복과 같다.
>

<br>

    tail call -> g 에서 f 를 호출하고 함수를 끝냄
              -> 컴파일러가 분석해서 g 를 안 거치고 호출자를 바로 이동한다.
              -> g 에 대한 Activation record 를 f 를 위한 것으로 재사용 leaf routine 과 비슷

<br><br><br>

## 8.4 Higher-Order Functions

### 8.4.1 The first-class functions

- declared within any block 
- passed as argments to other functions
- return as results of functions
- closure : representation of functions as value under static scoping. Note objects.

<br>

>
- 다른 어떤 블록 없이 선언되었다.
- 다른 함수에 함수로 넘겨진다.
- 함수들의 결과로 반환된다.
- 클로져 : 정적인 범위 안에 값으로서 함수들을 표시하는 것. 객체를 참고해라.
>

<br>

    The first-class function : 함수가 1급 객체인 것

    클로져는 정리가 조금 더 필요하겠지?

    Closure (내포체) : { 함수코드 ref + Activation record ref }

<br><br><br>

### 8.4.2 Passing Functions to Functions

Closure is a pair of
- pointer to function code
- pointer to an activation record

<br>

>
클로져는
- 함수를 가리키는 포인터와
- 활성화 레코드를 가리키는 포인터의 쌍이다.
>

<br><br><br>

### 8.4.3 Returning Functions from Nested Scope

Upward funarg problem.
```js
function compose (f, g) { return x => g(f(x)); }
```
Closure with the activation record of the caller of compose is returned.

<br>

>
상향 funarg 문제 

합성 함수의 caller 의 활성화 레코드의 클로져가 반환된다. (???)
>

<br>

    Upward funarg problem : 함수가 closure로 전달될 때 Activation record 를 stack 처럼 회수할 수가 없는 문제

<br><br><br>

```js
function make_counter(init) {

    let count = init;
    fuction counter (inc) { count = count + inc; return count; }
    return counter;
}
let c = make_counter(1);
c(2) + c(2);
```

- Closures are used to set the access pointer when a function returned from a nested scope is called
- When functions are returned from nested scopes, activation records do not obey a stack discipline.
- Garbage collector can deallocated activation records that are no longer used.

<br>

>
- 함수가 중첩된 범위에서 호출되었을 때 접근 포인터를 설정하기 위해 클로져가 쓰인다.
- 함수가 중첩된 범위들에서 반환되었을 때, 활성화 레코드는 스택 규범을 따르지 않는다.
- GC 는 더이상 쓰지 않는 활성화 레코드들을 풀어준다.
>