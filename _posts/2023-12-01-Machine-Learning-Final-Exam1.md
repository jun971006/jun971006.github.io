---
title:  "[BigData] 머신러닝 - Kaggle - Iowa Housing Dataset 분석1"

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
- 정규화가 필요하겠다..!

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
이 단계에서는 결측치를 처리해준다.

우선 결측치가 있는 데이터를 뽑아보자

```r
# 데이터의 결측치를 확인해보자
NAcol <- which(colSums(is.na(Housing)) > 0)  # 모든 결측치 변수 생성
                                             # 결측치를 가지고 있는 변수는 총 25개이다.
sort(colSums(sapply(Housing[NAcol], is.na)), decreasing = TRUE) # 결측치 변수 별로 내림차순 정렬

# Pool.QC   Misc.Feature          Alley          Fence   Fireplace.Qu   Lot.Frontage  Garage.Yr.Blt    Garage.Qual 
# 2917           2824           2732           2358           1422            490            159            158 
# Garage.Cond    Garage.Type  Garage.Finish      Bsmt.Qual      Bsmt.Cond  Bsmt.Exposure BsmtFin.Type.1 BsmtFin.Type.2 
# 158            157            157             79             79             79             79             79 
# Mas.Vnr.Area Bsmt.Full.Bath Bsmt.Half.Bath   BsmtFin.SF.1   BsmtFin.SF.2    Bsmt.Unf.SF  Total.Bsmt.SF    Garage.Cars 
# 23              2              2              1              1              1              1              1 
# Garage.Area 
# 1   
```

- 데이터셋이 2930 by 82인데 결측치가 너무 많은 변수는 제외하고 나머지 변수들만 처리해보자
- 결측치 처리는 기존에 Kaggle에서 똑똑한 분들이 해놓은 자료를 참고해서 진행해봤다.

> link : https://www.kaggle.com/code/maestroyi/house-prices-prediction-with-r-to-korean/report

#### (1) Lot.Frontage
Lot.Frontage 변수는 쉽게 말해서 토지와 도로 사이의 거리라고 보면 된다. 
이 값은 동네(neighborhood)에 가격이 다 다르기 때문에, 
동네(neighborhood)별로 중위값을 구해서 결측치를 처리해보자.

```r
Housing <- Housing %>%
  group_by(Neighborhood) %>% # Neighborhood로 그룹핑 한 뒤
  mutate(Lot.Frontage = ifelse(is.na(Lot.Frontage), median(Lot.Frontage, na.rm = TRUE), Lot.Frontage)) %>% 
  # mutate를 사용해서 Lot.Frontage의 값을 중위수로 바꿔준다.
  ungroup()
```

#### (2) Garage
다음으로는 Garage 관련 변수들을 처리해보자.
Garage 관련 변수로는  Garage.Yr.Blt, Garage.Qual, Garage.Cond, Garage.Type, Garage.Finish, Garage.Cars, Garage.Area 총 7개다.

##### (2) - 1 Garage.Yr.Blt
우선 Garage.Yr.Blt 변수는 차고가 지어진 년도로 건물이 지어진 년도(Year.Built)로 대체하겠다.

```r
Housing <- Housing %>%
  mutate(Garage.Yr.Blt = ifelse(is.na(Garage.Yr.Blt), Year.Built, Garage.Yr.Blt))
```

##### (2) - 2 Garage.Qual
Garage.Qual 변수는 차고의 품질을 나타내는 변수로,
Garage.Qual과 SalePrice간에 품질별로 값이 순서가 있는것을 확인할 수 있었다.

결측치는 차고가 없다는 'None'으로 대체한다.

순서가 존재하는 데이터이기 때문에 순서형 벡터를 활용해서 품질별로 순서를 매겨줬다.

```r
Housing$Garage.Qual[is.na(Housing$Garage.Qual)] <- 'None' # 결측치 처리

# 순서형 벡터 변수를 만들어주자
Qualities <- c('None' = 0, 'Po' = 1, 'Fa' = 2, 'TA' = 3, 'Gd' = 4, 'Ex' = 5)

# Qualities를 활용해서 integer 형으로 변환해준다.
Housing$Garage.Qual <- as.integer(revalue(Housing$Garage.Qual, Qualities))
```

##### (2) - 3 Garage.Cond
Garage.Cond 변수도 Garage.Qual 변수와 마찬가지로 순서가 존재하고,
결측치는 'None'으로 처리한 뒤, 순서형 벡터로 순서를 매겨준다.

```r
Housing$Garage.Cond[is.na(Housing$Garage.Cond)] <- 'None' # 결측치 처리

# 마찬가지로 순서가 존재한다.
Qualities <- c('None' = 0, 'Po' = 1, 'Fa' = 2, 'TA' = 3, 'Gd' = 4, 'Ex' = 5)

# Qualities를 활용해서 integer 형으로 변환해준다.
Housing$Garage.Cond <- as.integer(revalue(Housing$Garage.Cond, Qualities))
```

##### (2) - 3 Garage.Type
Garage.Type 변수는 차고의 종류를 나타내는 변수로, 따로 순서가 보이지 않는다.
결측치를 'None'으로 처리해주고, factor형 변수로 바꿔주자

```r
Housing$Garage.Type[is.na(Housing$Garage.Type)] <- 'No Garage' # 결측치 처리

Housing$Garage.Type <- as.factor(Housing$Garage.Type) # factor형으로 변환환
```

##### (2) - 4 Garage.Finish
우선 변수 내에 비어있는 값을 처리해주자
```r
which(Housing$Garage.Finish == '') # 비어있는 값이 보인다.

# 1357번째 row가 범인이다.
Housing$Garage.Finish[1357] <- 'Unf' # 최빈값인 Unf로 대체해주겠다.
```
데이터 확인 결과 순서가 있네? 이제 설명없이 처리하겠다..
```r
Housing$Garage.Finish[is.na(Housing$Garage.Finish)] <- 'None' # 결측치 처리

# Qualities 벡터 생성하기
Qualities <- c('None' = 0, 'Unf' = 1, 'RFn' = 2, 'Fin' = 3)

# Qualities를 활용해서 integer 형으로 변환해준다.
Housing$Garage.Finish <- as.integer(revalue(Housing$Garage.Finish, Qualities))
```

##### Garage 결측치 처리 확인
결측치 처리 후에 잘됐는지 결과를 확인해보자..!
만약 아직 남아있다면 추가로 처리해주자
```r
summary(Housing[, c('Garage.Yr.Blt','Garage.Qual', 'Garage.Cond','Garage.Finish', 'Garage.Type', 'Garage.Cars', 'Garage.Area')])
```

#### (3) Basement
다음으로는 지하실 관련 변수들의 결측치를 처리해보자
이녀석도 관련 변수가 상당히 많다..

```r
# Bsmt.Qual, Bsmt.Cond, Bsmt.Exposure, BsmtFin.Type.1, BsmtFin.Type.2
#    79         79             79             79             79
# Bsmt.Full.Bath, Bsmt.Half.Bath, BsmtFin.SF.1, BsmtFin.SF.2, Bsmt.Unf.SF, Total.Bsmt.SF 총 11가지의 종류가 존재한다.
```
지하실 관련 변수들도 차고 관련 변수들을 처리했듯이 결측치를 처리한 뒤, <br>
순서가 존재하면 순서형 벡터를 통해 integer형 변수로 변환하고, 없다면 factor형으로 변환한다.

##### (3) - 1 Bsmt.Qual
Bsmt.Qual은 지하실의 품질을 나타내는 변수이고, 순서를 가지고 있다..!
어떻게 아냐고? SalePrice 랑 그래프 그려서 보여주겠다.

![](/assets/images/bigdata/final/SalePrice_Bsmt_Overall_Quality_histogram.png)

보이는 것과 같이 "Ex", "Gd", "TA", "FA", "NA", "Po" 순으로 SalePrice값이 작아지는데,
"FA"랑 "NA"가 거의 비슷하니까 같은 비중을 주자

```r
# 결측치는 지하실이 없다고 판단한다.
Housing$Bsmt.Qual[is.na(Housing$Bsmt.Qual)] <- 'None' # 결측치 처리

# Qualities 벡터 생성하기
Qualities <- c('None' = 1, 'Po' = 0, 'Fa' = 1, 'TA' = 3, 'Gd' = 4, 'Ex' = 5)

# Qualities를 활용해서 integer 형으로 변환해준다.
Housing$Bsmt.Qual <- as.integer(revalue(Housing$Bsmt.Qual, Qualities))
```

##### (3) - 2 Bsmt.Cond
다음으로는 Bsmt.Cond 변수를 처리해보자
마찬가지로 순서를 가지고 있으니까, 순서형 벡터로 처리해주자!

```r
Housing$Bsmt.Cond[is.na(Housing$Bsmt.Cond)] <- 'None' # 결측치 처리

# Qualities 벡터 생성하기
Qualities <- c('None' = 0, 'Po' = 1, 'Fa' = 2, 'TA' = 3, 'Gd' = 4, 'Ex' = 5)

# Qualities를 활용해서 integer 형으로 변환해준다.
Housing$Bsmt.Cond <- as.integer(revalue(Housing$Bsmt.Cond, Qualities))
```

##### (3) -3 Bsmt.Exposure
Bsmt.Exposure 변수는 순서가 존재하지는 않지만,
히스토그램으로 데이터를 관측해봤을 때, "Av" 등급의 분포와 가장 유사하다.

```r
# 해당 값들을 'Av'로 대체해준다.
Housing$Bsmt.Exposure[which(Housing$Bsmt.Exposure == '')] <- 'Av' # 결측치 처리
```

##### (3) - 4 BsmtFin.Type.1
BsmtFin.Type.1 변수는 지하 마감재의 유형을 나타냅니다.
이 변수도 순서가 존재해 보인다.

 GLQ  Good Living Quarters
 ALQ  Average Living Quarters
 BLQ  Below Average Living Quarters   
 Rec  Average Rec Room
 LwQ  Low Quality
 Unf  Unfinshed
 NA   No Basement

```r
Housing$BsmtFin.Type.1[is.na(Housing$BsmtFin.Type.1)] <- 'None' # 결측치 처리

# Qualities 벡터 생성하기
Qualities <- c('None' = 0, 'Unf' = 1, 'LwQ' = 2, 'Rec' = 3, 'BLQ' = 4, 'ALQ' = 5, 'GLQ' = 6)

# Qualities를 활용해서 integer 형으로 변환해준다.
Housing$BsmtFin.Type.1 <- as.integer(revalue(Housing$BsmtFin.Type.1, Qualities))
```

##### (3) - 5 BsmtFin.Type.2
BsmtFin.Type.2 변수도 BsmtFin.Type.1 변수와 비슷하게 처리하자

```r
Housing$BsmtFin.Type.2[is.na(Housing$BsmtFin.Type.2)] <- 'None' # 결측치 처리

# Qualities 벡터 생성하기
Qualities <- c('None' = 0, 'Unf' = 1, 'LwQ' = 2, 'Rec' = 3, 'BLQ' = 4, 'ALQ' = 5, 'GLQ' = 6)

# Qualities를 활용해서 integer 형으로 변환해준다.
Housing$BsmtFin.Type.2 <- as.integer(revalue(Housing$BsmtFin.Type.2, Qualities))
```

##### (3) - 6 나머지 Bsmt 변수 처리
다음으로 Bsmt.Full.Bath, Bsmt.Half.Bath, BsmtFin.SF.1, BsmtFin.SF.2, Bsmt.Unf.SF, Total.Bsmt.SF 변수의 결측치를 처리해보려고 한다.

해당 row들은 지하실이 없는 경우이기 때문에 0으로 대체해준다.

```r
Housing$Bsmt.Full.Bath[is.na(Housing$Bsmt.Full.Bath)] <- 0
Housing$Bsmt.Half.Bath[is.na(Housing$Bsmt.Half.Bath)] <- 0
Housing$BsmtFin.SF.1[is.na(Housing$BsmtFin.SF.1)] <- 0
Housing$BsmtFin.SF.2[is.na(Housing$BsmtFin.SF.2)] <- 0
Housing$Bsmt.Unf.SF[is.na(Housing$Bsmt.Unf.SF)] <- 0
Housing$Total.Bsmt.SF[is.na(Housing$Total.Bsmt.SF)] <- 0
```

#### (4) Mas.Vnr.Area
Mas.Vnr.Area 변수를 처리해보자,

- 해당하는 row의 type을 None으로 대체해준다.

```r
Housing$Mas.Vnr.Type[which(Housing$Mas.Vnr.Type == '')] <- 'None'
```

- 이 변수는 재료의 타입을 가지고 있고, 재료별로 SalePrice의 분포가 차등하다.

```r
ggplot(Housing, aes(x = Mas.Vnr.Type, y = SalePrice)) + # ggplot으로 막대 그래프 생성
  geom_bar(stat = 'summary', fill = "#8da0cb") + # 요약 통계
  scale_y_continuous(breaks = seq(0, 400000, by = 100000), labels = comma) + # y축 구분선을 표현합니다. 0 ~ 800000
  geom_label(stat = 'count', aes(label = ..count.., y = ..count..)) # 관측치의 개수를 표현합니다.
```

여기까지 변수들의 결측치를 처리해봤으니, 분석에 사용할 변수를 선택해보자.

### 2) Feature Selection
변수를 선택하는 방법은 정말 다양하지만, 그 중에서 성능이 가장 좋았던 방법 몇 가지만 정리해보려고 한다.

- 우선 변수를 선택하기 전에 8:2로 학습용 데이터셋과 검증용 데이터셋으로 데이터를 나눠주자.
  - 여기서 데이터셋으로 사용되는 Housing_pre 데이터 프레임은 전처리가 완료된 Housing 데이터 프레임을 의미한다.

```r
# ★ 결측치 처리 후 Train, Validation 데이터 나누기 ★  
# numeric, factor형으로 데이터를 나눈다.
Housing_num <- Housing_pre[sapply(Housing_pre, is.numeric)]
Housing_fac <- Housing_pre[sapply(Housing_pre, is.factor)]

# numeric 데이터 - Train, Validate 데이터 프레임 나누기 - length(52)
set.seed(7)
num_trainIndex <- createDataPartition(Housing_num$SalePrice, p = .8, list = FALSE, times = 1)

trainset_num <- Housing_num[num_trainIndex,]
validationset_num <- Housing_num[-num_trainIndex,]

dim(trainset_num)      # 2346 by 52
dim(validationset_num) # 584 by 52
```

#### (1) Correlation > 0.5

- 첫 변수 선택 아이디어로는 변수간 상관계수 값이 0.5보다 큰 경우에만 선택해준다.

```r
# SalePrice와 상관관계 높은 변수 찾기
train_cor_numVar <- cor(trainset_num, use='pairwise.complete.obs') # 모든 Integer 변수의 상관 계수를 구한다.

# 상관계수 데이터에서 SalePrice 행을 추출한 후 
# sort함수로 정렬해준 뒤 as.matrix함수를 통해 행렬로 변환한다.
train_cor_sorted <- as.matrix(sort(train_cor_numVar[, 'SalePrice'], decreasing = TRUE)) 

# 상관 계수가 큰 변수만을 선택
train_CorHigh <- names(which(apply(train_cor_sorted, 1, function(x) abs(x) > 0.5))) # 정렬된 행렬에서 요소의 값이 0.6이상인 변수명만 저장한다.
train_cor_numVar <- train_cor_numVar[train_CorHigh, train_CorHigh] # 위에서 찾은 상관계수가 높은 변수만 가지고 행렬을 만든다.

corrplot.mixed(train_cor_numVar,   # corrplot 함수를 사용해서 변수들 간의 상관관계 플롯을 생성합니다. 
               tl.col = 'black',   # 변수명의 색은 검정색
               tl.pos = 'lt',      # 변수의 이름은 왼쪽에 표시합니다.
               number.cex = 0.6)   # 플롯 내부 상관계수의 글씨크기를 설정합니다.

```

- 상관계수 확인 결과 종속변수인 SalePrice와 14개의 독립변수, 총 15개의 변수를 선택해서 분석을 진행해보자..!


### 3) Data Transforms

to be continue...
> [다음글 보기](https://jun971006.github.io/bigdata/Machine-Learning-Final-Exam2)

<hr>
