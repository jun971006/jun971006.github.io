---
title:  "[BigData] 빅데이터분석론 - R Crash Course"

categories:
  - BigData
tags:
  - BigData
  - R
  - Planning
last_modified_at: 2023-03-27T01:16:00-05:00
published: false

toc: true
toc_sticky: true
toc_label: "목차"
---


### R Crach Course 1

#### 1. R 기초와 데이터마트
R 기초
데이터마트
결측치, 이상치처리

#### 2. 통계분석
통계학 개론
기초통계분석
다변량분석
시계열예측

#### 3. 정형데이터 마이닝
분류분석
군집분석 - 클러스터링 특징으로
연관분석 - 장바구니 분석이라고도 한다..?


#### 4. 비정형데이터 마이닝
텍스트마이닝
사회연결망 분석 - 네트워크
 -> 영향력의 크기를 알 수 있다.


### R Crach Course
 R 특성, R 스튜디오 환경 이해  
 프로그램 구조 및 요소 이해,  변수 종류, 선언
 데이터 구조, 데이터 가져오기, 데이터 분석 기초, 데이터 가공
 데이터 전처리, EDA, 데이터 시각화 개요


> shines39.tistory.com


메모리에 데이터를 올려놓고
그걸 사용하기 위해
데이터 구조를 배운다.

#### R 데이터 구조
- 같은 형태의 데이터
    - 스칼라
    - 벡터
    - 행렬
    - 배열

- 데이터 타입 : numeric, character, factor(범주 : 성별 등)

- 데이터프레임 : 서로 다른 유형의 데이터형 저장
    - 열(column) : 변수

- 리스트 : 벡터, 행렬, 데이터프레임 각각 다 가지고 있을 수 있다.


다변량 : 변수가 여러개


벡터 : 같은 타입의 변수를 가지고 있는 일차원배열

데이터 프레임
``` R
> str(df1)          # structure(구조)   
'data.frame':	5 obs. of  3 variables:
 $ x: int  1 2 3 4 5
 $ y: num  2 4 6 8 10
 $ z: chr  "홍길동" "임꺽정" "성춘향" "이도령" ..
```


- 행을 obs observation이라하고 
- 열을 variable이라고 하는 구나....

열은 $로 접근가능


as.factor : 변수 타입 변환함수

함수 정리 필요
- 중앙값과 평균 차이****

sum(is.na) : 결측치가 몇개?


### R Crash Course 2


#### 1. File(Excel, csv) 읽어서 DF으로 데이터 처리

- 워킹 디렉토리 설정
  - getwd()
  - setwd()

- 라이브러리 다운로드
  - sqldf
    - sql만 알고 R은 모를 때 사용
  - dplyr
  - ggplot2
    - grammars of graphics 

- excel , csv, xls 
  - csv 용량이 훨씬 작다!!

- 히스토그램 = 도수분포표

- 데이터를 가지고 노는 행위 = 먼징

- boxplot 에서 동그라미 -> 이상치

- 통계용어/개념
  - 확률밀도함수
  - 정규분포
  - 신뢰구간
  - 표준편차
  - 분산

- ifelse
```R
mpg2$grade <- ifelse(mpg2$ave >= 20, "P", "F" ) 
```

- filter -> 원하는 행을 추출하는 것

- a = 5 -> a에 5를 할당(파라미터)
- == 조건
- 파이프라인 연산자 : %>%

- %>%
```R
mpg2 %>% filter(grade == "P") # 결과값이 메모리에만 존재
mpg_pass <- mpg2 %>% filter(cyl >= 6 & grade == "P")   
# 엔드 조건 &, mpg_pass에 6기통 이상, pass 조건 만족하는 데이터 출력
```
- arrange()
  - 순서대로 정렬
  - 디폴트는 오름차순

- 예제
  - 등급 : p이고, 메이커별 모델별로 그룹하고, 그룹의 평균연비 계산하고, 연비 순서대로 메이커, 모델, 연비 출력

```R
mpg2 %>% 
  filter(grade == "P") %>%
  group_by(maker, model) %>%
  summarizes(model_ave = mean(ave)) %>%
  select(c(maker, model, model_ave)) %>%
  arrange(desc(model_ave))
```

- 통계 -> 추리(론)통계, 기술통계
  - 기술통계(Descriptive Sstatistics) : 수집한 데이터를 요약, 묘사, 설명하는 통계기법
  - 추리통계(Inferential statistics) : 데이터를 바탕으로 추론, 예측하는 통계기법

- 결측치?
  - NA
  - 데이터가 중간중간 비어있는 것


- complete.cases(df1)
  - 행에 대한 NA값 존재 여부 확인


- ggplot 
  - 그리고 싶은 데이터를 차곡차곡 쌓아서 그릴 수 있다.

> fundamentals of graphics
> bookdown.org 참고

- '23. 4. 3.(월) 과제
  - https://shines39.tistory.com/entry/ggplot%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EA%B0%80%EC%8B%9C%ED%99%94-I
  - https://shines39.tistory.com/entry/ggplot%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EB%8D%B0%EC%9D%B4%ED%84%B0%EA%B0%80%EC%8B%9C%ED%99%94-II
  읽어오기

  > '23. 9. 11.(월) 과제
  > 블로그 글 직접 따라서 복습하기
  