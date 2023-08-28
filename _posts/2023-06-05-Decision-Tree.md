---
title:  "[BigData] 빅데이터분석론 - 의사결정나무"

categories:
  - BigData
tags:
  - Decision Tree
  - Ensemble
  - Random Forest
last_modified_at: 2023-06-05T19:10:00-05:00

toc: true
toc_sticky: true
toc_label: "목차"
---

### 의사결정나무
- 의사결정 규칙을 나무구조로 도표화하여 분류와 회귀를 수행하는 방법
- 회귀, 분류 둘 다 가능

- 말단 노드 : Terminal node = leaf node
- 중간 노드 : Intermediate node
- 뿌리 노드 : Root node
  - 분류 대상이 되는 최상위 노드
- 가지 분할(Split) : 트리의 가지를 분리하며 생성하는 과정
- 가지 치지(Pruning) : 생성된 가지를 잘라내어 모형을 단순화하는 과정

- 특징
  - 분류 및 회귀 유형 문제에 모두 적용 가능
  - 수치형, 명목형 변수 처리에 용이하고, 중요하지 않은 특성은 제외 가능
  - 사용자에게 결정 이유 설명하기 용이
  - 다양한 패키지가 있음
  - 모델이 과적합, 과소적합 되기 쉬움

    과적합(오버피팅) : 훈련 데이터는 잘맞추고, 실 데이터는 못맞춤
    ex) 모의고사는 잘보고 실고사는 잘 못본다 

- Tree 생성 개념
  - 전체 그룹을 소그룹으로 반복 분할(재귀분할)

  - 왼쪽 : 순수도가 낮다. 불순도가 높다.
  - 오른쪽 : 순수도가 높다. (한 종류로만 분류) 불순도가 낮다.
    - 상위노드의 분할기준에 의해 분기되는 하위 노드에서의 노드내 동질성이 커지도록(불순도(Impurity)가 작아지도록) 분류
  - 팩터형변수 지니(Gini) 지수 적용
    - 0 <= G <= 1/2
    - 지니 지수가 작게 분류하는것이 좋다.

  - 지니 지수가 가장 작은걸 찾아서 분류의 기준으로 삼는다.

  - Tree 가지치기(Pruning)

- 의사결정 트리 머신러닝 프로세스
  1. 데이터 준비
    - 전처리
    - 학습 / 검정 데이터로 나누고
  2. 의사결정 트리 생성(학습데이터로 생성)

  ```R
  dt_iris <- rpart(Species ~. , data = train_set)
  ```

  3. 예측 및 모델 평가(시험데이터)
   ```R
  predicted_result <- predict(dt_iris, newdata = test_set, type="class")
  ```

  4. 성능개선(파라미터 조정 등)


- 실습1 : 분류 나무

- 실습2 : 회귀 나무

- 앙상블 (Ensemble)
  - 함께, 동시에 라는 뜻, 
  - 여러모형의 예측결과를 종합하여 정확도를 향상시키는 방법

  - 여러가지 머신러닝 알고리즘을 사용한 뒤 투표해서 정하는 방법

  - ★★ Bagging : Bootstrap Aggregation
    - 병렬 앙상블 모델
    - 원 데이터 집합으로 부터 크기가 같은 표본을 여러 번 단순 임의 <strong>복원추출</strong>하여 각 표본(Bootstrap 표본)으로 학습하고 결과를 종합
    - out of bag sample 로 검정하면된다.
      - 복원추출해서 안나온 데이터들

    - Bootstrap sample로 학습하고 샘플마다 Decision Tree Model 생성 후 투표 = Bagging

  - Boosting
    - 연속된 앙상블 모형
    - 원 데이터 집합으로부터 표본을 뽑아 학습하고,
      재 표본과정에서 동일한 확률이 아닌, <strong>오분류한 데이터에 가중치를 주어 샘플링하여 학습 반복(순차적)</strong>

  
  - Random Forest(대표적 Bagging 방법)
    - Bias와 Variance
    - Variance : 퍼진정도 = 분산
      - (예측값 - 예측값의 평균) ^ 2
    - Bias : 편차
      - (예측값의 평균 - 실측값) ^ 2
    - 동시에는 잡을 수 없다...
    - 적당한 균형이 중요하다.. 균형 잡는것이 앙상블!!

    - 회색 : 예측값의 평균

    - 랜덤포레스트는 표본과 변수를 랜덤으로 선택
      1. bootstarpped data 생성
      2. Decision Tree 생성 
      3. 1 ~ 2 반복
      4. 만든 각각 트리로 새로운 데이터를 분류
      5. <strong>OOB(Out of Bag sample)</strong> 데이터 활용 랜덤포레스트 테스트
        - 통상 1/3

    - randomForest 
      - 파라미터
        - mtry = 변수 개수

    ```R
    > rf <- randomForest(Species ~., data = train_set)  
    > # 파라메터를 디폴트로하니, 간단하지요?
    > rf               # 랜덤포레스트 결과인 rf를 보자.  

    Call:
     randomForest(formula = Species ~ ., data = train_set) 
                   Type of random forest: classification
                         Number of trees: 500
    No. of variables tried at each split: 2
    
            OOB estimate of  error rate: 5.71%
    Confusion matrix:
               setosa versicolor virginica class.error
    setosa         34          0         0  0.00000000
    versicolor      0         32         2  0.05882353
    virginica       0          4        33  0.10810811
    ```


        
    RMSE

    

