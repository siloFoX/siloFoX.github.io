---
layout: post
title:  "AI framework Development 6"
date:   2020-09-08
excerpt: "전처리 지옥"
tag:
- AI
- framework
- blog
- silofox
comments: true
feature: https://github.com/siloFoX/siloFoX.github.io/blob/master/images/help/help.jpg?raw=true
---

# 기나긴 전처리

그놈의 전처리는 끝나질 않는다. 그래도 정리는 해야하니까..

gisais 데이터를 연다.<br>
clade 데이터가 없는 것을 제외한다.

### 데이터 개수 92086 -> 92050

<br>

OECD 데이터만 뽑는데 여기도 마찬가지로 이름이 다르다.<br>
"South Korea" 를 "Republic of Korea" 로<br>
"USA" 를 "United States of America" 로 바꾼다.

그리고 체코랑 영국 데이터가 없어서 drop 시켰다.<br>
나머지 OECD 35개국 데이터만 추출했다.

### 데이터 개수 92050 -> 44641

<br>

clade 데이터를 One_Hot encoding 했다.<br>
날짜를 기준으로 sorting<br>

날짜 데이터 중 YYYY-MM-DD 포멧에 안 맞는 데이터는 <br>
모두 마지막 날짜로 만들었다.<br>

이유 : 새로운 clade가 나라에 나타난 정보일 수도 있기도 하고<br>
월 말에 위치해놓으면 실제 날짜보다 일찍 나타나게 기록되지는 않기 때문이다.

2020 이라고 되어있는 경우는 아직 해를 넘지 않았으므로 그냥 삭제한다. <br>
2020-MM 의 경우 그 달의 말일으로 치부한다.<br>
2020-01-XX 의 경우도 말일로 치부한다.

### 데이터 개수 44641 -> 44198

<br>

만약 2020-01-01 에 A clade 가 발병했으면<br>
2020-02-01에도 있을 것이라 가정하고,<br>
2020-02-01의 clade 정보에도 A 를 기록했다.
(각 나라에 독립적으로 적용했다.)

데이터를 살펴본 뒤 모든 clade 가 나타나면 그 이후에는 의미가 없어지므로 <br>
[ 1, 1, 1, 1, 1, 1, 1 ]<br>
나라별로 이런 꼴이 1번 이상 나오면 뒤에 날짜는 다 삭제하기로 했다.

### 데이터 개수 44198 -> 5676

<br>

마찬가지로,<br>
[1, 1, 0, 0, 0, 1, 0]<br>
[1, 1, 0, 0, 0, 1, 0]<br>
이렇게 되어있는 경우도 날짜만 다르고 의미 없기 때문에<br>
중복성 검사를 통해 삭제한다.

<br>

### 데이터 개수 5676 -> 199

마지막으로 국가를 기준으로 정렬하고(알파벳 순서)<br>
국가는 그대로 두고 날짜를 기준으로 오름차순 정렬한다.

<br>

## 결과

indentation 더럽게 안 맞지롱 ~ ㅋ
```
        Country	    G	GH	GR	L	O	S	V	Date
0	Australia	0	0	0	1	0	0	0	2020-01-22
1	Australia	0	0	0	1	0	1	0	2020-01-24
2	Australia	0	0	0	1	1	1	0	2020-01-24
3	Australia	1	0	0	1	1	1	0	2020-01-25
4	Australia	1	0	1	1	1	1	0	2020-01-27
5	Australia	1	0	1	1	1	1	1	2020-03-06
6	Australia	1	1	1	1	1	1	1	2020-03-09
7	Austria	0	0	1	0	0	0	0	2020-02-24
8	Austria	0	0	1	1	0	0	0	2020-02-26
9	Austria	0	0	1	1	1	0	0	2020-03-03
10	Austria	1	0	1	1	1	0	0	2020-03-03
11	Austria	1	0	1	1	1	0	1	2020-03-06
12	Austria	1	1	1	1	1	0	1	2020-03-07
13	Austria	1	1	1	1	1	1	1	2020-03-16
14	Belgium	0	0	0	0	0	1	0	2020-02-03
15	Belgium	0	1	0	0	0	1	0	2020-02-29
16	Belgium	1	1	1	1	0	1	0	2020-03-01
17	Belgium	0	1	1	1	0	1	0	2020-03-01
18	Belgium	0	1	0	1	0	1	0	2020-03-01
19	Belgium	1	1	1	1	1	1	0	2020-03-05
20	Belgium	1	1	1	1	1	1	1	2020-03-07
21	Canada	0	0	0	1	0	0	0	2020-01-23
22	Canada	0	0	0	1	1	0	0	2020-02-16
23	Canada	0	1	0	1	1	0	0	2020-02-28
24	Canada	1	1	0	1	1	1	0	2020-02-29
25	Canada	0	1	0	1	1	1	0	2020-02-29
26	Canada	1	1	1	1	1	1	1	2020-03-05
27	Canada	1	1	1	1	1	1	0	2020-03-05
28	Chile	0	0	0	0	0	1	0	2020-03-02
29	Chile	1	0	0	0	0	1	0	2020-03-05
...	...	...	...	...	...	...	...	...	...
169	Slovenia	1	0	0	0	1	0	1	2020-03-29
170	Slovenia	1	1	0	0	1	0	1	2020-04-19
171	Spain	1	0	0	0	0	0	0	2020-02-25
172	Spain	1	0	0	0	0	1	0	2020-02-26
173	Spain	1	0	1	0	0	1	0	2020-02-27
174	Spain	1	0	1	0	0	1	1	2020-02-28
175	Spain	1	0	1	0	1	1	1	2020-03-02
176	Spain	1	1	1	0	1	1	1	2020-03-03
177	Spain	1	1	1	1	1	1	1	2020-03-05
178	Sweden	0	0	0	0	1	0	0	2020-01-31
179	Sweden	0	0	0	1	1	0	0	2020-02-26
180	Sweden	0	0	1	1	1	0	0	2020-03-02
181	Sweden	1	0	1	1	1	0	0	2020-03-03
182	Sweden	1	1	1	1	1	0	0	2020-03-07
183	Sweden	1	1	1	1	1	0	1	2020-03-11
184	Sweden	1	1	1	1	1	1	1	2020-04-14
185	Switzerland	1	0	0	0	0	0	0	2020-02-24
186	Switzerland	1	0	1	0	0	0	1	2020-02-26
187	Switzerland	1	0	1	0	0	0	0	2020-02-26
188	Switzerland	1	0	1	0	1	0	1	2020-03-09
189	Switzerland	1	1	1	0	1	0	1	2020-03-12
190	Switzerland	1	1	1	0	1	1	1	2020-03-16
191	Switzerland	1	1	1	1	1	1	1	2020-05-27
192	United States of America	0	0	0	0	0	1	0	2020-01-19
193	United States of America	0	0	0	0	1	1	0	2020-01-21
194	United States of America	0	0	0	1	1	1	0	2020-01-27
195	United States of America	0	0	0	1	1	1	1	2020-02-27
196	United States of America	1	0	1	1	1	1	1	2020-02-28
197	United States of America	1	0	0	1	1	1	1	2020-02-28
198	United States of America	1	1	1	1	1	1	1	2020-02-29

199 rows × 9 columns
```