---
date: '2025-11-11T01:14:19+09:00'
draft: false
title: 'DeepFM: A Factorization-Machine based Neural Network for CTR Prediction'
tags: ['DL', 'RecSys']
summary: BOAZ 미니프로젝트 2를 위한 발제 논문입니다.
---

> Huifeng Guo, Ruiming Tang, Yunming Ye, Zhenguo Li, & Xiuqiang He. (2017). **DeepFM: A Factorization-Machine based Neural Network for CTR Prediction.**

## Why this paper?

BOAZ에서 미니프로젝트 2를 진행하게 되었다.   
우리의 주제는 kaggle의 [eCommerce behavior dataset](https://www.kaggle.com/datasets/mkechinov/ecommerce-behavior-data-from-multi-category-store/data?select=2019-Oct.csv)을 활용해 추천 시스템을 개발하는 것이다. 데이터에는 유저 id와 interaction 유형, 그리고 상품의 카테고리와 브랜드, 가격에 대한 정보가 포함되어 있다. 이 데이터셋을 사용하면, 상품에 대한 다양한 정보가 포함되어 있기 때문에 추천 모델은 물론, 완성된 결과를 통해 사용자들의 특성 또한 분석해볼 수 있어 데이터에서 인사이트를 실제로 뽑아낼 수 있는 경험을 할 수 있을 것이라 생각했다. 또한, 데이터가 매우 크고 결측치도 많아서, raw한 실제 데이터를 다뤄볼 수 있다는 점도 흥미로웠다.

조금 얘기가 딴 곳으로 빠졌는데...ㅎㅎ 그래서 이 모델을 선정한 이유는 DeepFM이라는 모델이 **다양한 feature 간의 관계를 효과적으로 파악할 수 있다는 특성**을 가지고 있기 때문이다. 이 데이터셋 또한 앞서 말했듯이 feature가 다양하므로, 특징에 부합한다.   
또한, Recommendation System에 대한 지식이 팀원 모두가 부족한 상황이라, neural net을 사용한다면 **classic한 모델을 통해 해당 도메인에 대한 기초적인 문제 정의, 그리고 해결 방안을 학습**하는 것이 필요하다고 생각했다. 따라서 이 논문을 선정하였다. 

마지막으로, 이 논문은 CTR task를 다루고 있어 모든 recommendation을 포함하는 task를 수행하는 것은 아닌데, 그 부분은 우리가 하려는 것도 결국 유저가 이 상품을 누를 건지? 아닌지?를 판단하는 것이니, 크게 문제가 되지 않을 것이라고 생각했다. 일단은... 읽어보자...ㅎㅎ

(과거 CTR을 예측하는 프로젝트를 교내 AI/DS 학회 EURON에서 진행한 경험이 있다. 당사 발표자료는 [여기](https://file.notion.so/f/f/2d2ba728-10d4-4fba-9b1a-7df5952869c2/c54c9de2-facc-4b89-ba09-48262cd9caf9/CTRonauts_%EC%B5%9C%EC%A2%85_%EB%B0%9C%ED%91%9C.pdf?table=block&id=19d3a5b8-804c-805b-b946-cd9313aa6679&spaceId=2d2ba728-10d4-4fba-9b1a-7df5952869c2&expirationTimestamp=1762819200000&signature=A-XfzM2clcA8iNTpOqF_W6sXASLMwqufZl1a-DPl7tI&downloadName=CTRonauts+%EC%B5%9C%EC%A2%85+%EB%B0%9C%ED%91%9C.pdf))

## Abstract
- CTR task에서는 유저의 행동 속 **정교한 피쳐 간 상호작용**을 학습하는 것이 중요.
- But, 기존의 방법은 다음과 같은 문제점을 가짐.
  - 모델이 low-order (ex: FM), 또는 high-order(ex: neural nets)에만 **편향**됨.
    - low-order : 적은 feature들 간의 연관 (1, 2개), high-order : 많은 feature들 간의 연관 (7개, 8개...)
  - **정교한 feature engineering**의 필요성
- 따라서 우리는?
  - **DeepFM**이라는 모델을 고안.
    - 추천에 강한 FM과 feature learning에 강한 deep learning을 결합
    - input을 "wide"와 "deep" part로 나눔. (참고: [Wide & Deep Models](https://arxiv.org/abs/1606.07792))  
- 기존의 문제점은 다음처럼 해결함.
  - low-order, high-order feature 상호작용을 **모두 강조**
  - **feature engineering 필요 없음**
- 실제 실험을 통해서도 이 모델이 CTR prediction에 있어 benchmark, 그리고 실제 commercial한 data 모두에 대해서 효과적, 효율적으로 작동함을 확인했다!

## Introduction

### CTR 예측이란? & 중요한 점

- **CTR (Click-Through Rate) prediction** : 유저가 추천한 아이템을 클릭할 확률을 예측하는 task
- **CTR에서 중요한 점**
  - 유저의 클릭에서 숨겨진 **feature interaction을 학습**하는 것!
    - 이 때, **다양한 order의 feature interaction**이 존재하므로, 모두를 동시에 반영하면 성능이 올라감. (From. Wide & Deep model)
    - feature interaction의 대부분은 **data 속에 숨어 있고**, 관찰을 통해 의미를 파악하는 것이 어려움. 
      - ex: diaper & beer. 아빠들이 기저귀를 사면 함께 맥주를 사 간다는 의미인데, 겉으로만 봐서는 의미의 연관을 파악하기 어려움;
    - 따라서 machine learning을 통해 자동적으로 분석하는 것이 필요함!

### Previous approaches
- **FTRL (generalized linear models)** 
  - 장점
    - 단순하지만, 잘 작동
  - 단점
    - feature interaction을 잘 학습하지 못해서, feature vector에 pairwise feature interatction을 직접 넣어줘야 함.
    - 고차원/잘 등장하지 않는 feature interaction 일반화가 어려움.
  - **Factorization Machines**
    - 장점
      - pairwise feature interaction을 모델링 가능 (feature's latent vector 간 inner product로)
    - 단점
      - 고차원의 feature interaction을 모델링할 수 있지만, 실제로는 complexity 때문에 2차원까지만 사용.
  - DNN models **(CNN, RNN based)**
    - 장점
      - 정교한 feature interaction 학습이 가능.
    - 단점
      - inductive bias - CNN : 이웃한 feature간 interaction에 집중, RNN : sequential한 dependency에 집중.
  - **Factorization-machine supported Neural Network (FNN)**
    - 단점
      - FM의 성능에 모델의 성능이 제한됨.
      - 저차원 feature interaction을 잘 포착하지 못함.
  - **Product-based Neural Network (PNN)**
    - 단점
      - 저차원 feature interaction을 잘 포착하지 못함.
  - **Wide & Deep**
    - 장점 
      - linear model (wide)과 deep model (deep)을 결합해 둘의 장점을 모두 취함.
    - 단점
      - 두 모델의 input을 다르게 요구.
      - wide part는 여전히 feature engineering을 요구.

  - 모델들이 가지는 주요 문제점
    1. 저차원 / 고차원 feature interaction에 **biased**됨.
    2. **feature engineering**에 의존
### Our approach

- **모든 차원**의 feature interaction을 학습하면서, **feature engineering이 필요하지 않은** 모델을 만들겠다!
- **주요 contribution**
  1. FM과 DNN을 결합해, 위의 장점을 가진 neural network **DeepFM**의 제안
  2. 효율적 training : wide part와 deep part의 input이 같기 때문. -> Wide & Deep과의 차별화
  3. benchmark / commercial data에 모두 평가 -> CTR prediction에 있어 일관된 성능 향상을 확인.

## Our Approach

### Problem Setting

- **n**개의 (X, y)
  - X : user & item pair data record, **m-fields**
    - 범주형 + 연속형 변수 포함.
    - one-hot encoding or 그대로
  - y : {0 - not clicked, 1 - clicked}
- CTR prediction : 유저가 주어진 맥락에서 특정 앱을 선택할 확률을 측정하는 모델 만들기
