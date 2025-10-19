---
date: '2025-10-18T19:09:23+09:00'
draft: false
title: 'Naive Bayes & SVM 구현하기'
tags: ["ML"]
description: 기계학습 세 번째 과제를 풀어봅니다.
---

## Naive Bayes란?

**input feature**와 **각 class**의 **joint probability**를 모델이 학습하고, 가장 **가능성 높은 클래스를 추측**하는 방법이다.

그러면, 여기서 우리는 이런 질문을 할 수 있다.

1. 상관관계를 모델에 어떻게 학습시킬 것인가?
2. 모델은 어떻게 클래스를 추측할 것인가?

이 부분을 더욱 살펴보겠다.

### 모델을 어떻게 학습시킬까?

Naive Bayes는 왜 naive할까?


