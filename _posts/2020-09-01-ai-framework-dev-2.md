---
layout: post
title:  "AI framework Development 2"
date:   2020-09-01
excerpt: ""
tag:
- AI
- framework
- blog
- silofox
comments: true
feature: https://github.com/siloFoX/siloFoX.github.io/blob/master/images/help/help.jpg?raw=true
---

우선 데이터 정제부터<br>
짜놨던 전처리 코드를 실행한다.

# 데이터 정제 

정제라고 해봤자 FEP 에서 처리하는 것처럼 그냥 가벼운 전처리이다.<br>
column 들을 drop 하고 text type 의 json data 로 GISAID 와 WHO 데이터를 처리한다.<br>
데이터 견본은 밑에서 전처리 할 때 하나씩 뜯어보도록 하겠다.

그리고 바로 
dataset/covra/1st-dataset/gisaid,  dataset/covra/1st-dataset/who 에 각각 데이터를 넣었다.<br>
GISAID data는 COVID-19의 계통군 정보, 장소 시간<br>
WHO data는 COVID-19의 나라별 발병률, 치사율, 시간 

각각 데이터는 사이트에서 Crawling 해서 얻은 data 이다.<br>
