---
title:  "[AI] 생성 AI"

categories:
  - AI
tags:
  - AI
  - BigData

last_modified_at: 2024-03-06T01:16:00-05:00

published: false

toc: true
toc_sticky: true
toc_label: "목차"
classes: wide
---
### 1주차

근본적으로 생성..? 발생

데이터의 근원지가 무엇인지 탐구하는 AI

모델을 만든다 => 

데이터의 근원 = 데이터의 분포

"문장" 보다 더 큰 근원 => "언어"

문장을 샘플로 보여줘서 그에 대한 근원인 언어를 찾아내자


### 2주차

#### ML/DL Basics
- ☆ 퍼셉트론 : 인간의 뇌처럼 생각하게 

- Linear Models : 직선 형태로 문제를 해결
  - XOR : 직선을 어디에 그어도 구분할 수 없음 -> 해결X
    -> 곡선으로 해결 가능


- 성별로 키를 구분할 때
 - 추가적으로 몸무게를 추가하면 더 잘 구분
 - 계속해서 데이터를 추가하면 더 잘 구분할 수 있지 않을까...?
 - 생물학적인 데이터로 체지방률 추가
   - 데이터가 없을 떄 근사할 순 없을까??
     - BMI로 해결

- Unit 개수 == 모델의 크기

#### 

비선형으로 문제를 해결한다 -> 복잡해진다.

- MLP : Multi-Layer Perceptron

- MLP를 훈련시키는 방법
  - Feed Forward
  - Back Propagation
  
####

CNN : 이안 굿펠로우

인간이 사물을 식별하는 방법
 - 구역을 나눠서 보고,
 - 구역은 정보를 나타내고,
 - 정보는 숫자로 표현 가능하고,
 - 숫자는 벡터로 표현해서

 - 벡터가 모여서 최종 정보

 - 이걸 CNN으로 풀겠다.

 

 ### 중간고사

 - 퍼셉트론
  - 데이터를 가지고 선형결합하여 새로운 데이터를 만들고
    그 데이터들을 통해서 y를 맞추는 것

 데이터를 조합해서 남/여를 더 잘 구분 - 차원을 확장한다.

 - 1969년에 발명되었는데 안쓰였다.. 그 이유??
    - Feed Forward
    - back propagation

 - Gradient Vanishing   
    - 망각의 원리
      - ReLu를 Sigmoid 대신에 사용하기 시작함
        - Sigmoid 함수를 사용하면 중요하지 않은 것들도 
          망각되지 않아서 중요한게 뭔지 모르게 됨
        - ReLu를 도입해서 중요하지 않은 것들은 지워지게 함

    - DropOut
      - 데이터의 일부를 강제로 없앤다.

#### 3주차
- 시계열 데이터 
  -> 통계적 관점에서 "시간"이라는 속성에 의해 발생하는 문제

- 데이터의 분포가 Non-Stationarity 
   -> 구간의 평균이 다르다.

- RNN(Recurrent Neural Network)의 문제점 : 그래디언트 소실
  -> LSTM(Long-Short Term Moemory)이 나옴

  - 은닉층 메모리 셀
    + 입력, 망각, 출력 게이트 추가
    - 셀 상태 : 입력 게이트와 삭제 게이트의 출력 이용 계산

#### 4주차
- CNN(Convolutional Neural Network)
  -> 이미지를 인식하는 문제를 뉴럴넷으로 푼다면?

  - 합성곱신경망
    -> 데이터의 내제된 Local한 특징을 추출하는 역할
       - 구간정보가 중요

- p16. Multivariate, Univariate
  - Univariate   : 시간에 따른 관측치가 1개인 데이터
  - Multivariate : 시간에 따른 관측치가 N개인 데이터

#### 5주차

- p13. 분석 방법

- 그레인저 인과관계(Granger Causality)
  - 한 변수의 변화가 시차를 두고 다른 변수에 영향을 주는 관계
  - 시차를 바꿔가며 회귀분석을 수행
  - 진정한 의미의 인과관계는 아님
    -> 수식을 보면 상관관계에 가까움
    -> 인과관계 != 상관관계
       -> 회귀분석을 했을 때 통계적으로 그럴 확률이 높다 
          -> 상관관계가 크다 != 인과관계가 크다

- 공적분 관계(Cointegration) = 동행관계
  -> 선형결합을 했을 때 Stationary한 시계열이 되는 관계

- p14. 그레인저 - 선후관계(선행 - 후행)
  -> 시차를 두고

- p17. 공적분 - 시계열이 동행

#### 7주차
- GAN(Generative Adversarial Networks)
  -> 실제 데이터의 분포에 가까운 데이터를 생성

- Deep Learning 2.0이란 ..? 
   -> out-of-sample
      - 샘플 너머의 무언가를 캐치하고 학습하는 것

- Latent Space(잠재 공간)가 핵심 ★★★
  - 안보여준 샘플 = 모집단 특징
  - 진짜 데이터의 분포를 근사하는 벡터 ★★★

- GAN이 어떻게 학습이 되느냐? 
  - Latent Space에서 받아서 Fake Sample 만들고
    - Fake를 Fake로 맞춘다(Discriminator가)
      -> Fake를 Real에 가깝게 만든다. (컨셉)
 
  - Fake를 Real, Real을 Fake라고 분류했을 떄
    Discriminator의 Loss가 커진다.

  - Fake를 Fake라고 분류했을 때
    Generator의 Loss가 커진다.

  - 최종적으로 Real에 가까운 샘플을 생성하게끔 만드는 네트워크가 GAN
  - Real 샘플과는 다른 모집단이 생성될 확률이 높다. = 라이선스 걱정X
    -> Generator가 Real Sample을 참고하지 않았기때문에





### 기말 고사

#### 9주차
- Attention 모델
  - 어디에 집중? 이 핵심
  - 왜 쓰지? Interpretability 
    - 각 입력에 대해 중점을 둘 수 있음
  - 매우 긴 문장 -> Attention 씀
    (Long Range Dependency 문제 핸들 가능)
  - QKV -> query - key - value가 중요

#### 10주차
- GPT (문제 많이 나옴)
  - Transformer 기반 알고리즘
    - 내부적으로 Attention 쓰인다. (핵심)
  - Generative Model이다. (핵심)
    - Explicit, Implicit으로 나뉜다.
  - GPT는 Explicit -> 데이터의 분포를 알아야함
  - p16 - 가장 중요
    - 언어의 분포를 정의(추정)한다.
    - 앞에 단어들이 주어졌을 때, 바로 뒤에 올 단어가 주어질 확률
    - 앞 문맥을 통해 뒤 단어의 확률 계산
  - GPT1에서 바뀐게 많이 없고, 모델의 크기만 키운것..


#### 11주차
- BERT
  - Bidirectional이 핵심
  - p7 - 가장 중요
    - GPT는 앞에만 봄
    - 왜 양방향 안봄? 힘듦
      - 양방향 수식을 정의하기 어려움
    - BERT는 Implicit 모델
  - MLM (Masked Language Model)
    - Bidirectional
    
- Electra 
  - MLM중에 가장 고도화된 모델
  - p8 - 실험 결과가 가장 중요
    - GPT랑 비교했을 때 
      - 같은 파라미터 대비 성능이 좋음

#### 
- Signal 은 안함

####
- Stable Diffusion
  - 어떤것을 가정하느냐
  - p16 - 중요
    - 노이즈로부터 데이터를 만드는건 쉽다.
      - 그게 generative modeling 이다.

  - Diffusion models
    - 자연계 데이터 = x + 정규분포(노이즈)
      - 노이즈의 분포는 정의할 수 있다. = 정규분포
      - 더하고 빼면서 원본 데이터를 가질 수 있다.
      - Im, Ex 중간에 있는 모델이다.

####
- Prompt Engineering
  - Decoding Strategies가 중요
    - 생성모델은 확률값을 구하는 것 = 샘플링
    - 확률값을 높이는 것 = 프롬프트 엔지니어링의 본질
  - p18이 가장 중요

####
- Lang Chain 
  - p12
    - 따로따로 만드는게 좋음
    - 원하는 답을 내놓게 하는것이 중요
      - ex) 번역 전문가 + 보고서 전문가
        - 각각의 체인을 만들어서 수행하면 Task가 쉬워짐
          - 모델이 작더라도 결합해서 사용하면 충분히 좋음
        
    