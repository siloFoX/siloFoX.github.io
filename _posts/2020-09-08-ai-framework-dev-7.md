---
layout: post
title:  "AI framework Development 7"
date:   2020-09-08
excerpt: "데이터 정리와 요구사항"
tag:
- AI
- framework
- blog
- silofox
comments: true
lastmod : 2020-09-18
sitemap : 
  changefreq : never
  priority : 1.0
feature: https://github.com/siloFoX/siloFoX.github.io/blob/master/images/help/help.jpg?raw=true
---

크게 처리된 데이터와 처리되지 않은 데이터가 있다.


# 처리되지 않은 데이터

## GISAID 데이터 

```
strain               
virus                
gisaid_epi_isl       
genbank_accession    
date                 
region               
country              
division             
location             
region_exposure      
country_exposure     
division_exposure    
segment              
length               
host                 
age                  
sex                  
pangolin_lineage     
GISAID_clade         
originating_lab      
submitting_lab       
authors              
url                  
title                
paper_url            
date_submitted    
```

<br>

## WHO 데이터 

```
Country              
Country_code         
Cumulative_cases      
Cumulative_deaths     
Date_reported        
New_cases             
New_deaths            
WHO_region    
```

<br>
--- 
<br>

# 처리된 데이터

## GISAID 데이터 

```
GISAID_clade         
age                  
country              
country_exposure     
date                 
division             
division_exposure    
gisaid_epi_isl       
location             
pangolin_lineage     
region               
region_exposure      
strain               
```

<br>

## GISAID clade 데이터

```
Country            
GISAID_clade_G     
GISAID_clade_GH    
GISAID_clade_GR    
GISAID_clade_L     
GISAID_clade_O     
GISAID_clade_S     
GISAID_clade_V     
Date               
```

<br>

## Country 데이터

```
Country
Country_code
Population
Populcation_density
Average_wage
```

<br>
<br>

아래의 데이터를 KSB에서 AWS 백엔드로 전송할 수 있어야 한다.

## 개괄

1. 확진자, 사망자 단순통계
    - 해당일 기준 전세계 + 나라별 전체 사망자, 전체 확진자
    - 전세계 + 나라별 날짜별 사망자, 확진자 (WHO 데이터 그대로 쓰면 될듯)
2. 예측 확진자, 사망자
    - 예측 가능한 기간까지의 날짜별 전세계 + 나라별 사망자, 확진자
3. Clade 통계
    - (S, L, V, G, GR, GH, 기타) 총 7개 클레이드 각각에 대하여
        - (해당일 기준 합신) 보고된 개수, 사망률, 최초 발생지, 최초 발생 시각 (전세계 + 나라별)<br>
         → 도넛 그래프에 사용함
4. Clade 통계 및 예측
    - 각 클레이드별 날짜별 보고된 개수, 사망률(선형 그래프 그릴때 사용)
    - 각 클레이드별 날짜별 예측 가능한 기간까지의 보고된 개수, 사망률(선형 그래프 그릴때 사용)


## Schema

JSON 형식으로 KSB가 쏴주는 식으로 된다. 그래서 하루마다 한번씩 gzip으로 압축된 데이터를 줘야 한다.<br>
객체 → JSON str → Gzip binary → base64 str → http body 로 가야함을 명심해야 한다

```c++
//누적은 백엔드가 알아서 처리하도록
{
	death: {
		global: {
			from: "2020-01-01",
			to: "2020-09-08"
			data : [
				1,
				1,
				// 이하 일별 사망자 수
			]
			//이하 35개국 데이터
		},
	},

	confirmed: {
		from: "2020-01-01",
		to: "2020-09-08",
		global: {
			data : [
				1,
				1,
				// 이하 일별 확진자 수
			]
			//이하 35개국 확진자 데이터
		},
	},

	deathPrediction: {			
		from: "2020-01-01",
		to: "2020-12-31" //이것은 변동될 수 있고, 미래일것임
		global: {
			data : [
				1,
				1,
				// 이하 일별 예측 사망자 수
			]
			//이하 35개국 데이터
		},
	},
    //여기부터 clade
	cladePopulation: {
		global: { //보고된 개수
			label: ['S', 'L', 'V', 'G', 'GR', 'GH', 'Other'] //순서 정의
			data: [5326, 3975, 4775, 18946, 29562, 19464, 3685]
		},
		//이하 각 35개국 데이터
	}

	cladePopulationPrediction: {
		global: {
			//TODO 미래의 우리가 할거야
		}
	}
}
```