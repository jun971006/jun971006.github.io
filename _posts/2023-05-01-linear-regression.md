---
title:  "[BigData] 빅데이터분석론 - 회귀분석"

categories:
  - BigData
tags:
  - BigData
  - Linear Regression
last_modified_at: 2023-05-01T19:10:00-05:00

toc: true
toc_sticky: true
toc_label: "목차"
---


### 회귀분석

- 통계분석
    - 상관분석(Corrlelation Test)  
    - 회귀분석(Regression)
    - 교차분석(Test)
    - paired sample test
    - anova

- 기술통계
- 추론통계(추리)

> R을 이용한 통계 데이터분석

- 교차검정(카이 검정)

- y^ : y hat : 예측값
- y- : y bar : 평균값
- y  : y     : 실측값

- 잔차(오차, Residuals) : 예측값과 실측값의 차

- 단순회귀
- 다중회귀 : 여러 값을 예측하기 위해 여러 변수를 사용한다.

- 근사하게 만드는것이 회귀식이다.

- 오차들을 다 더하면 0이된다.
    - 오차들을 제곱한다.
        - 제곱의 합 : Sum of Square Error : SSE
        - 면적의 합을 구하는 것
        - 오차의 합을 최소화하게 하는 값을 찾는 것 ==> 회귀식

- 최소자승(제곱)법
    - 에러들을 제곱한 값을 다 더한다.
    - 더한값이 가장 작게 만드는 값이 최소자승법
    - 오차항이 정규분포를 따른다.
    - 설명변수와 종속변수 사이에 선형관계가 성립한다.
    - 각 관측치들은 서로 독립한다.
    - 종속변수 y에 대한 오차항은 설명변수 값의 범위에 관계없이 일정하다.
        - 값이 작을 때만 분석이 잘되고, 클 때 안되는 경우

- 결정계수(R Square) : 예측을 얼마나 정확히 했는지를 나타내는 계수
  - 0과 1사이의 값을 나타낸다.
  - 1에 가까울수록 적합도가 높다.

- SST 평균, 실제값과의 차이
- SSE, SSR

- 회귀식이 잘못된게 아니라 예측을 하려면 다른 변수가 필요하다.
- Adjusted R Square 수정 결정계수

- p-value : 귀무가설이 맞다는 가정하에 그 귀무가설이 맞을 확률
  - p-value가 작다 -> 회귀분석이 유의미하다.

- runif : Random - Uniform
  - 균등분포하는 랜덤 숫자를 만든다.

- 회귀분석을 할 때 factor형 변수를 빼고하면 안된다.
  - 변수를 dummy化 한다.


###

```R
fit2 <- lm(formula = Sepal.Length ~ ., data = dat)

summary(fit2) 

plot(fit) 
# 1번 : 잔차의 등분산성 --> 잔차와 추정값이 특별한 규칙이 없어야..
# 2번 : 데이터의 정규성 확인 그래프 : Q-Q plot
# 3번 : Scale Location Plot 잔차의 표준화도 등분산 
# 4번 : Residual vs Leverage 빨강추세선의 0인게 이상적, 아웃라이어 확인가능 
# Cook's distance : 이상치와 같은 녀석들, 많으면 회귀분석에 방해가 된다.
```

- 최소의 변수로 설명하는것이 중요하다.
- 차원축소 : Dimensionality Reduction
- R square 값이 줄더라도 차원을 축소

- 차원 감소 기법
  - 변수중에서 몇 가지 변수를 선택하는 방법
  - 변수를 통한 회귀분석 식으로 변수를 추출

- 전역 탐색법
- 전진 탐색법
- 후진 소거법
- 단계적 선택법(전진 + 후진)

- 변수선택 평가지표 1 & 2
  - Akaike Information Criteria(AIC)
    - 작을수록 좋다.

- 범주형 데이터의 처리
  - 0 과 1이 의미가 생긴다.
    - 원핫인코딩(둘 중에 하나는 값이 1 하나는 0)
      - 범주형 데이터를 dummy化 

- 값에 의미가 들어가면 안된다.

- 범주형 변수의 더미 변수화부터는 자습
