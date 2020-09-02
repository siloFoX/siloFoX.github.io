---
layout: post
title:  "AI framework Development 2"
date:   2020-09-01
excerpt: "데이터 전처리 방안"
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

# IFTTT

같이 하는 친구가 만들어준 것들 : <br>
각각 데이터는 사이트에서 Crawling 해서 얻은 data 이다.<br>
IFTTT에 있는 Webhook을 사용했다. trigger server에서 Webhook을 불러서<br>
action server에 데이터를 넘겨준다. <br>
원래는 If Trigger then Action 인데, Trigger된 김에 data까지 보내버리는 것


# 데이터 견본과 전처리

통계치와 데이터형태는 Jupyter로 직접 뜯어보면서 하는 중
아래 데이터 포멧에 해당하는 92,086 개의 데이터가 존재함

GISAID 데이터
```json
[{
	"GISAID_clade": "GH", 
	"age": "33", 
	"country": "Australia", 
	"country_exposure": "Australia", 
	"date": "2020-08-15", 
	"division": "South Australia", 
	"division_exposure": "South Australia", 
	"gisaid_epi_isl": "EPI_ISL_516551", 
	"location": "", 
	"pangolin_lineage": "B.1.113", 
	"region": "Oceania", 
	"region_exposure": "Oceania", 
	"strain": "Australia/SAP488/2020"
},
...
]
```