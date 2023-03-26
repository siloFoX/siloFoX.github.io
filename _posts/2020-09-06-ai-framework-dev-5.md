---
layout: post
title:  "AI framework Development 5"
date:   2020-09-06
excerpt: "학습 데이터 쌍 생성"
tag:
- AI
- framework
- blog
- silofox
comments: true
lastmod : 2020-09-06
sitemap : 
  changefreq : never
  priority : 1.0
feature: https://github.com/siloFoX/siloFoX.github.io/blob/master/images/help/help.jpg?raw=true
---

# 6에서 싹 정리했으니 이건 거의 무시해도 될 듯

<br>
<br>
<br>

# 첫 모델을 위한 전처리

프레임워크 내에서 할 수 있는지 일단 모르기 때문에 그냥 내가 짜는 중<br>
(왜냐면 오퍼레이터가 94개나 되기 때문이다 ㅠ 너무 많아)

1. clade, country, date 만 뽑아낸다.
2. 빈 clade 를 제외한다.
3. country : OECD 만 뽑아낸다.
4. clade 를 one_hot 처리한다.
5. date가 뒤에 있는 것들은 기존 clade 의 정보를 덮어쓴다.

<br>

# One_Hot 이후에

예를 들면 

```
date            clade 
2020-09-05    0 1 0 0 0
```

```
date            clade
2020-09-06    0 0 1 0 0
```
이런식으로 데이터가 생성되어 있을 것이다.

그걸 

```
date            clade
2020-09-06    0 1 1 0 0
```

이런식으로 계속 이어지게 만드는 것이다.<br>
물론 나라 별로 다르게 할테지만 말이다.

순서는 이런식으로 한다.
1. date를 기준으로 정렬
2. dictionary를 만든다. (key = country, value = dictionary (key = clade, value = 1/0 ))
3. 한줄씩 실행하면서 df에 존재하는 clade 값을 dictionary clade에 1으로 대입하고, dictionary에 있는 clade 값을 df에 일괄 반영한다.