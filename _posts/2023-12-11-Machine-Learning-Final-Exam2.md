---
title:  "[BigData] 머신러닝 - Kaggle - Iowa Housing Dataset 분석2"

categories:
  - BigData
tags:
  - BigData
  - Machine Learning
  - Kaggle
  - 
last_modified_at: 2023-12-11T01:16:00-05:00

toc: true
toc_sticky: true
toc_label: "목차"
---

> [이전글 보기](https://jun971006.github.io/bigdata/Machine-Learning-Final-Exam1)

<hr>


## 4. Evaluate Algorithms 

### 1) Test options and Evaluation metric

모델 훈련 리샘플링 방법으로 반복 교차 검증 기법을 사용하고,
평가 척도로는 RMSE를 사용합니다.

train 효과를 높이기 위해 center, scale, boxcox 변환을 적용해서 전처리를 진행해줍니다.

```r
trainControl <- trainControl(method = "repeatedcv", number = 10, repeats =3)
metric <- "RMSE"
pre_parms <- c("center", "scale", "BoxCox")
```

위에서 정의한 옵션들을 활용해서 학습을 진행합니다.<br/>
시도해볼 알고리즘은 총 5가지입니다.
- Linear Algorithms : LM, Generalized LR (GLM)
- Nonlinear Algorithms : CART, SVM, KNN

```r
set.seed(7)
tic() # 시작 시간 재보기

#LM
fit.lm <- train(SalePrice~., data = trainset_num_after_cor, method = "lm", metric = metric,
                preProc=pre_parms, trControl = trainControl) 

# GLM
set.seed(7)
fit.glm <- train(SalePrice~., data = trainset_num_after_cor, method = "glm", metric = metric,
                 preProc=pre_parms, trControl = trainControl) 

# SVM
set.seed(7)
fit.svm <- train(SalePrice~., data = trainset_num_after_cor, method = "svmRadial", metric = metric,
                 preProc=pre_parms, trControl = trainControl) 

# CART 
set.seed(7)
fit.cart <- train(SalePrice~., data = trainset_num_after_cor, method = "rpart", metric = metric,
                  preProc=pre_parms, trControl=trainControl)

# KNN
set.seed(7)
fit.knn <- train(SalePrice ~., data = trainset_num_after_cor, method = "knn", metric = metric,
                 preProc=pre_parms, trControl = trainControl)

toc() # 끝 시간 재보기
```



### 2) Spot-Check Algorithms

### 3) Compare Algorithms

```r
results_rf_num_cor <- resamples(list(LM=fit.lm_rf_num_cor, GLM = fit.glm_rf_num_cor,
                                 SVM = fit.svm_rf_num_cor, KNN = fit.knn_rf_num_cor))
summary(results_rf_num_cor)
par(mfrow = c(1,3))
scales <- list(x=list(relation = "free"), y=list(relation="free"))
dotplot(results_rf_num_cor, scales = scales)
```

## 5. Improve Accuracy
### 1) Algorithm Tuning

### 2) Ensembles


## 6. Finalize Model

### 1) Predictions on validation dataset

### 2) Create standalone model on entire training dataset

### 3) Save model for later use

## Pre-processing

