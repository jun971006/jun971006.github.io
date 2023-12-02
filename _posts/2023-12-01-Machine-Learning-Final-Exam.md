---
title:  "[BigData] 머신러닝 - Kaggle - Iowa Housing Dataset 분석"

categories:
  - BigData
tags:
  - BigData
  - Machine Learning
  - Kaggle
  - 
last_modified_at: 2023-12-01T01:16:00-05:00

toc: true
toc_sticky: true
toc_label: "목차"
---

## 데이터 분석 처음부터 끝까지..!
교수님 : 주제 아무거나 정해서 처음부터 끝까지 데이터 주물럭거려봐..!

대학원 기말과제로 데이터 분석을 하게 되었다..

주제 선정, 데이터 전처리, 학습, 분석... 해야될게 많지만 하나씩 천천히 해보자

구글에 검색해서 최대한 레퍼런스를 많이 참고하면서 진행하려고 한다.

R Script를 사용해서 분석을 진행할 예정이고, 과제만 하면 나중에 까먹을 듯 해서 기록해 놓는다.

## 주제 - Iowa Housing Data Regression 
주변 환경요소에 따른 집값 추이 예측하기..!

> Link : https://www.kaggle.com/datasets/marcopale/housing

총 6가지의 큰 줄기로 분석을 진행할 예정이다.

## 1. Prepare Project
프로젝트 준비 단계이다.

### 1) Load Packages
첫 번째로 사용할 패키지를 로드해준다.

```r
library(caret)
library(corrplot)
library(dplyr)
library(scales)
library(ggrepel)
library(plyr)
```

### 2) Load Dataset
두 번째로 사용할 데이터셋을 로드해준다.

```r
getwd() # 작업 디렉토리 확인
setwd("D:\\R\\final") # 작업 디렉토리 세팅
Housing <- read.csv("AmesHousing.csv") # 데이터셋 로드

dim(Housing)
summary(Housing) 
str(Housing)

# AmesHousing Data 구성 확인하기 : 2930 obs. of  82 variables
# SalePrice Column을 포함해서 82개의 Column을 가지고 있다.
```

> 변수 참고 Link : https://mazdah.tistory.com/883

### 3) Split-out Validation Dataset
```r
set.seed(7) # seed 설정
Train_Index <- createDataPartition(Housing$SalePrice, p =0.8, list = FALSE) 
# 80%를 학습 데이터로 사용하기 
# caret 패키지 함수 사용

validation <- Housing[-Train_Index,] # 테스트용 데이터 프레임 생성
dataset <- Housing[Train_Index,]     # 훈련용 데이터 프레임 생성
```

## 2. Analyze Data
### 1) Descriptive Statistics
```r
# 훈련용 데이터 분석
dim(dataset)   # 훈련용 데이터는 2346 by 82 
head(dataset)  
str(dataset)    
summary(dataset)  # 곳곳에서 결측치가 관측된다..

int_vars <- sapply(dataset, is.integer)    # integer인 경우 변수
char_vars <- sapply(dataset, is.character) # character인 경우 변수

num_int_vars <- sum(int_vars)   # integer인 경우 더하기
num_char_vars <- sum(char_vars) # character인 경우 더하기

cat("Integer 변수 개수 : ", num_int_vars, "\n")
cat("Character 변수 개수 :", num_char_vars, "\n")
# integer형변수는 39개
# Character형 변수는 43개 .. 총 82개의 변수가 존재한다.
```

### 2) Data Visualization
- 데이터를 비주얼라이즈 해보자.
- 히스토그램으로 SalePrice 데이터를 시각화해봤을 때 오른쪽으로 치우친 것을 확인할 수 있다.
```r
ggplot(data = dataset, aes(x = SalePrice)) +
  geom_histogram(fill =  "#8da0cb", binwidth = 10000) +
  scale_x_continuous(breaks = seq(0, 800000, by = 100000), labels = comma) 
```
![](/assets/images/bigdata/final/SalePrice_Histogram.png)

- 다음으로 변수들간에 상관관계를 확인해보자
- 상관관계를 확인하기 위해 Integer형 변수로만 이루어져 있는 데이터 프레임을 만들어줍니다.

```r
all_numVar <- Housing[, int_vars] 
# 위에서 구한 integer index 를 가지고 integer형 변수만 가지고 있는 데이터셋을 만든다.

cor_numVar <- cor(all_numVar, use='pairwise.complete.obs') 
# 모든 Integer 변수의 상관 계수를 구한다.

cor_numVar # 변수 간에 상관계수를 확인해보자..
           # SalePrices와 Overall.Qual 간의 상관계수가 약 0.8정도로 높게 관측된다.

cor_sorted <- as.matrix(sort(cor_numVar[, 'SalePrice'], decreasing = TRUE)) 
# 상관계수 데이터에서 SalePrice 행을 추출한 후 
# sort함수로 정렬해준 뒤 as.matrix함수를 통해 행렬로 변환한다.
# 상관 계수가 큰 변수만을 선택

CorHigh <- names(which(apply(cor_sorted, 1, function(x) abs(x) > 0.5))) # 정렬된 행렬에서 요소의 값이 0.5이상인 변수명만 저장한다.

cor_numVar <- cor_numVar[CorHigh, CorHigh] # 위에서 찾은 상관계수가 높은 변수만 가지고 행렬을 만든다.

corrplot.mixed(cor_numVar,         
# corrplot 함수를 사용해서 변수들 간의 상관관계 플롯을 생성합니다. 
               tl.col = 'black',   # 변수명의 색은 검정색
               tl.pos = 'lt',      # 변수의 이름은 왼쪽에 표시합니다.
               number.cex = 0.8)   # 플롯 내부 상관계수의 글씨크기를 설정합니다.
```

![](/assets/images/bigdata/final/correlation_over_0.5_.png)

## 3. Prepare Data

### 1) Data Cleaning

### 2) Feature Selection

### 3) Data Transforms

## 4. Evaluate Algorithms

### 1) Test options and Evaluation metric

### 2) Spot-Check Algorithms

### 3) Compare Algorithms


## 5. Improve Accuracy

### 1) Algorithm Tuning

### 2) Ensembles


## 6. Finalize Model

### 1) Predictions on validation dataset

### 2) Create standalone model on entire training dataset

### 3) Save model for later use

## Pre-processing


\