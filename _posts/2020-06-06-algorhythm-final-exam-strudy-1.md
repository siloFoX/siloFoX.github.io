---
layout: post
title:  "Algo-Rhythm study for final exam"
date:   2020-06-06
excerpt: "al"
tag:
- exam
- study
- blog
- silofox
comments: true
feature: https://github.com/siloFoX/siloFoX.github.io/blob/master/images/algo-rhythm/algo-rhythm-feature.jpg?raw=true
---

미완

최소 곱셈 문제

행렬 n 개를 곱하는데 필요한 원소단위 곱셈이 최소 횟수와 최소 횟구로 행렬 곱셈하는 순서를 구하는 문제이다.

```
입력 : n (행렬의 개수), 인덱스의 범위가 0부터 n까지인 정수배열 d
여기서 d[i-1] X d[i] 는 i 번째 행렬의 크기가 된다.

출력 : minmult (n 개의 행렬을 곱하는데 필요한 원소단위 곱셈의 최소 횟수) 
P (최적의 순서를 구할 수 있는 이차원 배열) 
여기서 P 의 행의 인덱스는 1부터 n-1까지 이고, 열의 인덱스는 1부터 n까지 이다. 
P[i][j] 의 값은 i 부터 j 까지의 행렬을 곱할 때 최적의 순서로 갈라지는 기점을 나타낸다.
```

p 에서 최적의 순서를 구하는 방법에 대해 알아보자.
p[2][5] = 4 라는 사실은 A2 부터 A5 행렬을 곱하는데 최적의 순서가
(A2 * A3 * A4) * A5
라는 인수분해로 나타남을 의미한다.

여기서 괄호 안의 행렬은 최적의 순서이다.    