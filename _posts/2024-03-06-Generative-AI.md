---
title:  "[AI] 생성 AI"

categories:
  - AI
tags:
  - AI
  - BigData

last_modified_at: 2024-03-06T01:16:00-05:00

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
  
