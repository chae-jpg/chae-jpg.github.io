---
title: "Machine Learning Image Classifier"
date: 2024-02-01T00:00:00+09:00
draft: false
description: "A deep learning model for image classification using TensorFlow and Keras"
categories: ["Projects"]
featured: false
thumbnail: "/images/projects/placeholder.svg"
stack: ["TensorFlow", "Keras", "Python", "NumPy"]
excludeFromArchive: true
---

## 프로젝트 개요

이 프로젝트는 TensorFlow와 Keras를 사용하여 이미지 분류를 위한 딥러닝 모델을 구축한 프로젝트입니다. CIFAR-10 데이터셋을 사용하여 10개 클래스의 이미지를 분류하는 모델을 개발했습니다.

## 모델 아키텍처

### CNN 기반 모델
- **Convolutional Layers**: 특징 추출을 위한 합성곱 레이어
- **Pooling Layers**: 차원 축소 및 과적합 방지
- **Dense Layers**: 최종 분류를 위한 완전연결 레이어
- **Dropout**: 과적합 방지를 위한 드롭아웃 레이어

### 모델 성능
- **정확도**: 85.2%
- **훈련 시간**: 약 2시간 (GPU 사용)
- **모델 크기**: 15.2MB

## 기술 스택

- **Python 3.9**
- **TensorFlow 2.8**
- **Keras**: 고수준 API
- **NumPy**: 수치 계산
- **Matplotlib**: 시각화
- **PIL**: 이미지 처리

## 데이터 전처리

```python
# 이미지 정규화
train_images = train_images.astype('float32') / 255.0
test_images = test_images.astype('float32') / 255.0

# 원-핫 인코딩
train_labels = to_categorical(train_labels, 10)
test_labels = to_categorical(test_labels, 10)
```

## 모델 구조

```python
model = Sequential([
    Conv2D(32, (3, 3), activation='relu', input_shape=(32, 32, 3)),
    MaxPooling2D(2, 2),
    Conv2D(64, (3, 3), activation='relu'),
    MaxPooling2D(2, 2),
    Conv2D(64, (3, 3), activation='relu'),
    Flatten(),
    Dense(64, activation='relu'),
    Dropout(0.5),
    Dense(10, activation='softmax')
])
```

## 학습 결과

### 훈련 과정
- **Epochs**: 50
- **Batch Size**: 32
- **Optimizer**: Adam
- **Loss Function**: Categorical Crossentropy

### 성능 지표
- **Training Accuracy**: 89.1%
- **Validation Accuracy**: 85.2%
- **Training Loss**: 0.34
- **Validation Loss**: 0.67

## 시각화

모델의 학습 과정과 성능을 시각화하여 분석했습니다:

- **학습 곡선**: 정확도와 손실 함수의 변화
- **혼동 행렬**: 클래스별 분류 성능
- **예측 결과**: 테스트 이미지에 대한 예측 시각화

## 도전과 해결

### 과적합 문제
- **문제**: 훈련 정확도와 검증 정확도 간의 큰 차이
- **해결**: Dropout 레이어 추가, 데이터 증강 기법 적용

### 성능 최적화
- **문제**: 모델 훈련 시간이 오래 걸림
- **해결**: GPU 가속 사용, 배치 크기 최적화

## 향후 개선 계획

- [ ] 더 깊은 네트워크 구조 실험 (ResNet, DenseNet)
- [ ] 데이터 증강 기법 고도화
- [ ] 하이퍼파라미터 튜닝 자동화
- [ ] 모델 앙상블 기법 적용

## 코드 저장소

전체 코드와 상세한 문서는 [GitHub 저장소](https://github.com/username/ml-classifier)에서 확인할 수 있습니다.