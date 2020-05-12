---
layout: post
title:  "Algo-Rhythm 백준 1065 한수 1"
date:   2020-04-21
excerpt: "brute force"
tag:
- c++
- algorithm
- algo-rhythm
- 백준
- brute force
- 브루트 포스
- jamiroquai
- blog
- silofox
comments: true
---

추천 노래는 Jamiroquai - [Virtual Insanity](https://youtu.be/4JkIs37a2JE)

피아노와 함께 펑키하게 깔리는 명곡<br>
틀어놓고 알고리즘 문제를 구경하자.<br>
첫 포스팅이라 메이저 한 곡으로 골랐다!
<br>
<br>

## 문제

어떤 양의 정수 X의 각 자리가 등차수열을 이룬다면, 그 수를 한수라고 한다.<br>
등차수열은 연속된 두 개의 수의 차이가 일정한 수열을 말한다.<br>
N이 주어졌을 때, 1보다 크거나 같고, N보다 작거나 같은 한수의 개수를 출력하는 프로그램을 작성하시오.

## 입력

첫째 줄에 1,000보다 작거나 같은 자연수 N이 주어진다.

## 출력

첫째 줄에 1보다 크거나 같고, N보다 작거나 같은 한수의 개수를 출력한다.

## 제한

시간 제한 : 2초 (약 2억번 계산 : 초당 1억번이라고 생각하면 편하다.)<br>
메모리 제한 : 128 MB (int 기준으로 3000만개 정도 : int 100 만개가 4MB 정도라 생각하면 편하다.)

## 링크

[1065번: 한수](https://www.acmicpc.net/problem/1065)
<br>
<br>

## 의식의 흐름

한수라는 개념을 처음 봤다. 그래서 일단 예시를 만들어보자.<br>
123 은<br>
2 - 1 = 1<br>
3 - 2 = 1<br>
이므로 한수 이다.
<br>
<br>

그리고 예제를 보게되면 한자리수랑 두자리수는 다 한수라고 하는 것 같다.<br>
입력이 100보다 작으면 그냥 출력하면 될 것 같다.<br>
1000은 한수가 아니다.
<br>
<br>

3자릿수라고 가정하고, 각 자릿수를 문자라고 생각해보자<br>
(string)ABC = "123" ( A = 1, B = 2, C = 3) 
<br>
<br>

B - A = C - B <br>
뭐 이런 식이다. <br>
물론 (뒷 자릿수) - (앞 자릿수) 가 아니고 (앞 자릿수) - (뒷 자릿수) 해도 상관은 없다.<br>
(A - B = B - C 이므로..)
<br>
<br>

그럼 의사코드를 작성해보자. (한수의 개수를 새는 프로그램)

```sh
N 입력

if (N < 100) : 
    return N;

else : 
    count 에 99 대입
    num 를 111 부터 N 까지 반복해서 대입 :  // N 이 111 보다 작으면 실행이 안되겠죠?

    if (num 가 한수인가?) : 
        count++;

return count;
```
<br>
<br>

틀 잡기

```c++
#include <iostream>

using namespace std;

bool is_han_number (int num) {

        // 한수인지 검사하는 함수
        // return 값으로 true, false를 반환한다.
}

int num_han_range (int N) {

        // 의사코드에 따라 1~N 에서 한수의 개수의 개수를 반환하는 함수
}

int main() {

    int N;
    cin >> N;
    cout << num_han_range(N); 

    return 0;
}
```

자릿수마다 뽑아내는건 2가지 방법이 생각난다.

1. integer to string 으로 변환
2. 10의 제곱수, 나머지와 나누기 연산자, integer type의 특징(소숫점 계산 x) 으로

그 중에 2번째 방법을 썼다. 일반적인 케이스는 다음에 생각해보자.

```c++
#include <iostream>

using namespace std;

bool is_han_number (int num) {

    int hundred_digits = num / 100;
    int ten_digits = (num % 100) / 10;
    int one_digit = num % 10;

    cout << hundred_digits << " " << ten_digits << " " << one_digit;
}

int num_han_range (int N) {


}

int main() {

    int N;
    cin >> N;
    // cout << num_han_range(N); 

    is_han_number(N);

    return 0;
}
```

확인하기 위해서 일단 is_han_number로 잘 출력되는지 확인한다.<br>
지금은 자릿수 분리만 짜봤다.
<br>
<br>

결과는 성공~<br>
123 넣으면 1 2 3 나오고<br>
20 넣으면 0 2 0 나온다.

```c++
#include <iostream>

using namespace std;

bool is_han_number (int num) {

    int hundred_digits = num / 100;
    int ten_digits = (num % 100) / 10;
    int one_digit = num % 10;

    if ((hundred_digits - ten_digits) == (ten_digits - one_digit))
        return true;
    else
        return false;
}

int num_han_range (int N) {


}

int main() {

    int N;
    cin >> N;
    // cout << num_han_range(N); 

    cout << is_han_number;

    return 0;
}
```

한수인 것을 검사하는 코드이다.<br>
0 ~ 99 일 때는 무조건 false (검사할 일이 없어서!)<br>
100 ~ 1000 은 한수만 true 인 것을 확인했다.
<br>
<br>

```c++
#include <iostream>

using namespace std;

bool is_han_number (int num) {

    int hundred_digits = num / 100;
    int ten_digits = (num % 100) / 10;
    int one_digit = num % 10;

    if ((hundred_digits - ten_digits) == (ten_digits - one_digit))
        return true;
    else
        return false;
}

int num_han_range (int N) {

    if (N < 100)
        return N;
    else {

        int count = 99;
        int num;
        for (num = 111; num < N + 1; num++)
            if (is_han_number(num))
                count++;

        return count;
    }
}

int main() {

    int N;
    cin >> N;
    cout << num_han_range(N); 

    return 0;
}
```

결과는 성공! <br>
1~99 넣으면 실행시간은 거의 없는 수준<br>
최악일 때 1000 넣으면 1000 - 111 + 1 = 890 번 실행된다.<br>
for문 하나니까  복잡도는 O(n) 이다.
<br>
<br>

## 의문점들

brute force란 무엇일까?<br>
string으로 처리하는 것은 어떻게 할까?<br>
3자릿수가 아니면 어떡하지?