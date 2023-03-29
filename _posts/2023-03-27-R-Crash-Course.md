---
title:  "[BigData] 빅데이터분석론 - R Crash Course"

categories:
  - BigData
  - R
tags:
  - BigData
  - R
  - Planning
last_modified_at: 2023-03-27T01:16:00-05:00

toc: true
toc_sticky: true
toc_label: "목차"
---


### R Crach Course

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

sum(is.na) : 결측치가 몇개?