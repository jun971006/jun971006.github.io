---
title:  "[BigData] 빅데이터분석론 - 다변량 분석"

categories:
  - BigData
tags:
  - BigData
  - Multivariate Data Analysis
last_modified_at: 2023-05-08T19:10:00-05:00

toc: true
toc_sticky: true
toc_label: "목차"
---


#### 다변량 분석

- 일반적으로 피어슨(Pearson) 상관계수가 많이 쓰인다.
- Cov(X,Y)

- cor : 상관계수

- attach(mtcars) -> '$' 안붙여도 된다.

- 상관계수 행렬
  - cor(mtcars[,1:4])

- x와 y가 상관이 있어? 없어?
  - 권위있게 결론 내주는 것★★★
    - 검정(test)★, 상관관계 결론이니깐 상관검정
      - cor.test(hp, disp) # 마력, 배기량 상관검정

  ```R
  cor.test(hp,disp)    # 상관검정

	Pearson's product-moment correlation

  data:  hp and disp
  t = 7.0801, df = 30, p-value = 7.143e-08
  alternative hypothesis: true correlation is not equal to 0
  95 percent confidence interval:
   0.6106794 0.8932775
  sample estimates:
        cor 
  0.7909486 
  ```

- 단 건의 테스트 : t.test

#### 주성분 분석

- 주성분 분석 : 변수를 새롭게 만들어서 변수를 줄이는 것
- 만일 n=50 이라면 새로운 변수 3개로 회귀식을 만들수 있다.

- 제일 많이 퍼진쪽으로 투사 : 분산이 가장 큰 방향을 축으로
  - 2차원 -> 1차원으로 차원을 줄일 때 (차원을 축소하는 것!)
  - 선형변화 : 안뭉치게 잘 퍼져있는 방향으로 축을 세우자

  
#### 주성분 분석 결과의 해석
- propotion of variance 

- 산점도를 한 번에 볼 때 pair 사용


상관관계가 없다 -> 상관계수가 0이다.

어반팝과 머더가 상관관계가 없다는 귀무가설을 기각할 수 없다.

상관관계가 0이라는 것을 기각할 수 없다.
-> 귀무가설이 유의미하다.


귀무가설을 기각한다 = 귀무가설이 틀렸다. (p-value가 0.05보다 작다.)
상관관계가 없다는 귀무가설은 틀렸다.

#### 주성분 분석 수행
- princomp

- cor = FALSE (Default)
  - covariance matrix를 사용한다.

```R
fit <- princomp(USA, cor = T)  # 주성분분석, 상관계수 행렬을 이용 
summary(fit)                   # 주성분분석 결과 요약 
loadings(fit)                  # 주성분 분석결과 출력 
?loadings

> summary(fit)
Importance of components:
                          Comp.1    Comp.2    Comp.3     Comp.4
Standard deviation     1.5748783 0.9948694 0.5971291 0.41644938
Proportion of Variance 0.6200604 0.2474413 0.0891408 0.04335752
Cumulative Proportion  0.6200604 0.8675017 0.9566425 1.00000000

Proportion of Variance
Cumulative Proportion(누적비율)

> fit$loadings                   # loadings를 바로 불러봐도 되지 않을까?

Loadings:
         Comp.1 Comp.2 Comp.3 Comp.4
Murder    0.536  0.418  0.341  0.649
Assault   0.583  0.188  0.268 -0.743
UrbanPop  0.278 -0.873  0.378  0.134
Rape      0.543 -0.167 -0.818     
```

- 회귀식 : Comp.1 = 0.536*Murder + 0.538*Assault + 0.278*UrbanPop + 0.543*Rape

- 주성분을 구해서 차원을 축소시키기 위해 회귀식을 만든다.
- 더 적은 변수를 사용하기 위해..

```R
plot(fit, type = "lines")      # 주성분 분산의 크기 그림 : Scree plot 
# 주성분의 분산의 감소가 급격히 줄어든다 --> 설명력이 떨어진다. 
# 주성분을 몇개로 할 것인가를 결정할때, 참고 
# 여기서는 변수 3개정도면 적당

fit$score              # 주성분으로 계산한 값 
biplot(fit)            # x축 y 축 같이 새로만든 comp1과 comp2 축을 기준으로 배치한 플롯  


```