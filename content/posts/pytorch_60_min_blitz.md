---
date: '2025-10-23T21:52:16+09:00'
draft: true
title: "Deep Learning with Pytorch: A 60 Minute Blitz"
tags: ["DL", "Pytorch"]
summary: "Deep Learning with Pytorch: A 60 Minute Blitz를 통해 Pytorch를 복습합니다."
---

## Motivation

Pytorch를 사용해 여러 프로젝트를 진행해 보았지만, 아직도 그 프레임워크의 과정이 익숙하지 못하다는 생각이 들었다.

그래서 이 참에 튜토리얼을 보면서 전반적인 Pytorch의 흐름을 다시 훑고, 문법을 복습하고자 한다.

참고한 튜토리얼은 Pytorch 공식의 다음 튜토리얼이다: [Deep Learning with Pytorch: A 60 Minute Blitz](https://docs.pytorch.org/tutorials/beginner/deep_learning_60min_blitz.html)

---

## Tensor

**Tensor**는 배열이나 행렬과 유사한 자료구조이고, Pytorch에서는 이를 모델의 input과 output을 표현하는 데 사용한다.

Pytorch의 tensor은 더 빠른 연산을 위해 GPU와 같은 가속기에서 돌릴 수 있다는 점을 제외하면, NumPy의 **ndarray**와 매우 유사하다. 따라서, NumPy가 익숙한 사람들은 이 부분을 건너뛰어도 된다고 하는데, 나는 복습하는 김에 이 부분도 같이 공부했다.

---

### Tensor Initialization

tensor는 다양한 방법을 통해 초기화가 가능하다.

**Directly from a data**

다음과 같이 데이터로부터 바로 텐서를 만들 수 있다. 이 때, 매개변수를 넘겨주지 않는다면 타입은 자동으로 추론된다.

```python
data = [[1, 2], [3, 4]]
x_data = torch.tensor(data)
```

**추가)** 물론, 다음과 같이 타입을 지정하는 것도 가능하다.

```python
x_data_specify = torch.tensor(data, dtype=torch.int64)
```

**From another tensor**

ndarray를 통해 tensor를 만들 수 있다. 

```python
np_array = np.array(data)
x_np = torch.from_numpy(np_array)
```

**From another tensor**

특별히 지정하지 않는 한, 새로운 tensor는 기존의 속성을 유지한다.

```python
x_ones = torch.ones_like(x_data) # retains the properties of x_data
print(f"Ones Tensor: \n {x_ones} \n")

x_rand = torch.rand_like(x_data, dtype=torch.float) # overrides the datatype of x_data
print(f"Random Tensor: \n {x_rand} \n")
```

**With random or constant values**

`shape`는 tensor 차원을 나타내는 튜플이다. 아래의 함수는 주어진 `shape`에 따라 tensor의 차원을 결정한다.

```python
shape = (2, 3,)
rand_tensor = torch.rand(shape)
ones_tensor = torch.ones(shape)
zeros_tensor = torch.zeros(shape)

print(f"Random Tensor: \n {rand_tensor} \n")
print(f"Ones Tensor: \n {ones_tensor} \n")
print(f"Zeros Tensor: \n {zeros_tensor}")
```

---

### Tensor Attributes

Tensor의 속성은 모양, 자료형, 어떤 기기에 저장되어 있는지를 표현한다.

```python
tensor = torch.rand(3, 4)

print(f"Shape of tensor: {tensor.shape}")
print(f"Datatype of tensor: {tensor.dtype}")
print(f"Device tensor is stored on: {tensor.device}")
```

Out:
```Text
Shape of tensor: torch.Size([3, 4])
Datatype of tensor: torch.float32
Device tensor is stored on: cpu
```

---

### Tensor Operations

Pytorch는 100개가 넘는 tensor 연산을 지원한다고 한다. 물론 그것들을 하나하나 다 살펴보는 것은 비효율적이기 때문에... 이번에는 튜토리얼에 언급된 메소드만을 보기로 한다. 

모든 메소드에 대한 레퍼런스는 다음 링크를 참조한다. [링크](https://docs.pytorch.org/docs/stable/torch.html)

이러한 연산들은 빠른 속도를 위해 GPU 위에서 수행될 수 있다. 다음 코드를 통해 텐서를 GPU로 할당한다.

```python
# We move our tensor to the GPU if available
if torch.mps.is_available():
  tensor = tensor.to('mps')
  print(f"Device tensor is stored on: {tensor.device}")
```

Out:

```text
Device tensor is stored on: mps:0
```

나는 맥북을 사용하고 있어서 cuda 대신 mps를 불러왔다. mps는 Metal programming framework를 사용할 수 있도록 하는 Pytorch의 device인데, Metal은 간단하게 MacOS의 CUDA라고 생각하면 된다. [공식문서](https://docs.pytorch.org/docs/stable/notes/mps.html)

