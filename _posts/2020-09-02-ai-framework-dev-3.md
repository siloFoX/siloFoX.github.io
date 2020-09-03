---
layout: post
title:  "AI framework Development 3"
date:   2020-09-02
excerpt: "모델 1 설계"
tag:
- AI
- framework
- blog
- silofox
comments: true
feature: https://github.com/siloFoX/siloFoX.github.io/blob/master/images/help/help.jpg?raw=true
---

# 만들고자 하는 모델 1

Regression 모델을 활용<br> 
clade (계통군)에 따라서 다른 모델을 형성한다.

독립변수 : 날짜<br>
종속변수 : 사망자 수 혹은 발병률

모델 1-1 : 단순한 Regression<br>
모델 2-2 : clade 값을 One-hot encoding을 통해서 넣은 통합 Regression

계통군끼리 서로 영향을 미친다면 1-2 모델이 더 좋을 것이지만<br>
일단은 쉬운 것부터 테스트 해보는 것이 바람직하다.

<br>

# 데이터 처리 방향

clade 별로 통계를 내고<br>
그 중에 가장 데이터가 많은 것을 기준으로 발병률을 조사한다.

<br>

# GISAID_clade

```
'': 36,
'G': 20632,
'GR': 31418,
'S': 5714,
'GH': 20609,
'O': 4295,
'L': 4267,
'V': 5115
```
이런 통계를 가진다.

빈 값은 36개 밖에 없으니 무시하는 것이 좋을 것 같다.<br>
GR 값이 가장 많으니 처음 모델을 구성하는 것이 좋을 것 같다.

퍼센트로 나타내자면 
```
'' : 0.04 %
'G' : 22.41 %
'GR' : 34.12 %
'S' : 6.21 %
'GH' : 22.38 %
'O' : 4.66 %
'L' : 4.63 %
'V' : 5.55 %
```
이런 비율이다.

<br>

# 문제점

나라별 Clade의 비율을 알 수 없다.<br>
다만 언제 이 Clade가 처음 나타났는지는 알 수 있다.(장소, 시간)

우리가 원하는 데이터는 Clade 에 따라 치사율과 발병률의 추이를 예측하는 것이다.<br>
발병자 중 특정 Clade가 차지하는 비율을 알 수 없다면 GISAID에 있는 데이터만으로<br>
알 수는 없으나 Regression을 돌려볼 수는 있을 것이다.

<br>

# 모델 설계

가장 쉬운 모델은 Clade가 처음 나타나고 n일 뒤 그 나라의 발병률, 치사율 예측은 가능하다.
나라가 너무 많으므로 OECD 회원국으로 통계를 산출한다.

입력값 : n일 뒤(100일을 기준으로 0.01 scale 독립변수), Clade 종류(One-Hot),<br>
그 Clade 가 처음 나타난 날짜(Date), 나라의 GDP(0~1, 최대값으로 나눠준 결과),<br>
나라의 인구(0~1, 최대값으로 나눠준 결과), 나라의 인구밀도(0~1, 최대값으로 나눠준 결과), <br>
Clade가 나타나기 직전 발병률(100명당 발병률 : 발병인원 / 인구 * 100)과 치사율(0~1)<br>
결과값 : 발병인원(반올림한 정수)과 치사율(0~1) (모델이 2개)

그리고 치사율 예측은 비교적 정확할 것으로 보인다.<br>
기존에 다른 Clade의 치사율은 시간이 지나도 비슷할 것이기 때문이다.

결론 : Clade 별 치사율을 산출해보는 것

<br>

# 개발과정

현재는 OECD 회원국 데이터를 기준으로 산출한다.

1. 나라별 GDP, 나라의 인구, 나라의 인구밀도를 구한다.
2. 나라별로 Clade의 발생시기를 구한다. (현재는 가장 데이터가 많은 GR로 한다.)
3. 나라별 Clade가 나타나기 직전 발병률과 치사율을 구한다.
4. 발병률과 치사율을 날짜에 Mapping 한다.
5. 데이터를 취합하여 학습 데이터로 만든다.

<br>

# 필요한 데이터 

1. dictionary : { Country Code : GDP }
2. dictionary : { Country Code : Population}
3. dictionary : { Country Code : Population Density }