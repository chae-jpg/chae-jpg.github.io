---
date: '2025-07-04'
draft: false
title: '점프 투 파이썬 - 조건문'
tags: ['Python']
summary: 파이썬 조건문 문법을 알아봅시다.
---

## 조건문 (if 문)

`if` 문은 조건에 따라 실행 흐름을 제어하는 구문이다.

### 기본 구조

if 조건문:
실행할 문장1
실행할 문장2

- `조건문`이 참(`True`)이면 들여쓴 문장들이 실행됨
- 들여쓰기는 **공백 4칸** 또는 **Tab**으로 통일해야 함

### 예시

money = True
if money:
print(“택시를 타고 가라”)  # 출력됨

### if ~ else 문

`else`는 조건이 **거짓(False)** 일 때 실행된다.

```
money = False
if money:
print(“택시를 타고 가라”)
else:
print(“걸어가라”)  # 출력됨
```

### if ~ elif ~ else 문

여러 조건 중 하나를 선택할 수 있다.

```
money = 2000
card = True

if money >= 3000:
print(“택시를 타고 가라”)
elif card:
print(“카드가 있으니 택시를 타고 가라”)  # 출력됨
else:
print(“걸어가라”)
```

- 조건을 **위에서부터 차례로 검사**하며,
- 첫 번째로 참인 조건이 실행되고, 나머지는 무시됨

### 조건에 사용되는 연산자

- **비교 연산자** : `==`, `!=`, `>`, `<`, `>=`, `<=`
- **논리 연산자** : `and`, `or`, `not`

예시:

```
x = 3
y = 5

if x < y and x != 0:
print(“조건 만족”)  # 출력됨
```

### 조건문에 `in`, `not in` 사용

```
pocket = [‘paper’, ‘cellphone’, ‘money’]

if ‘money’ in pocket:
print(“택시를 타고 가라”)
else:
print(“걸어가라”)



if ‘card’ not in pocket:
print(“카드가 없다”)
```

### pass 키워드

`if` 문에서 조건은 참이지만, **아직 구현할 내용이 없을 때** 사용한다.
```
pocket = [‘paper’, ‘money’, ‘cellphone’]

if ‘money’ in pocket:
pass  # 아무것도 하지 않음
else:
print(“카드를 꺼내라”)
```

### 조건부 표현식 

한 줄로 조건문을 작성할 수 있다.

‘참일 때의 값’ if 조건 else ‘거짓일 때의 값’

예시:
```
score = 70
message = “success” if score >= 60 else “failure”
print(message)  # 출력: success
```

좋아! 이번엔 위키독스 21번 문서 내용을 바탕으로,
while 문을 빠짐없이 정리한 마크다운 코드 블록이야. 그대로 복붙해서 사용하면 돼:

## 반복문 (while 문)

`while` 문은 **조건이 참인 동안** 블록을 계속 반복 실행한다.

### 기본 구조

while 조건문:
반복 실행할 문장

조건문이 `True`인 동안 반복된다. 조건이 `False`가 되면 종료된다.

### 예시: 1부터 10까지 출력

```
i = 1
while i <= 10:
print(i)
i = i + 1
```

### while 문에서 break 사용

`break` 문은 반복문을 **강제로 종료**할 때 사용된다.

```
coffee = 10
money = 300

while money:
print(“돈을 받았으니 커피를 줍니다.”)
coffee -= 1
print(f”남은 커피는 {coffee}개입니다.”)
if coffee == 0:
print(“커피가 다 떨어졌습니다. 판매를 중지합니다.”)
break
```

### while 문에서 continue 사용

`continue` 문은 **남은 문장을 건너뛰고 조건 검사로 바로 넘어감**.

```
a = 0
while a < 10:
a += 1
if a % 2 == 0:
continue
print(a)  # 홀수만 출력됨
```

### 무한 루프 만들기

`while True`를 사용하면 무한 루프를 만들 수 있다. 반드시 `break` 등으로 종료 조건을 만들어야 함.

```
while True:
user_input = input(“종료하려면 q를 입력하세요: “)
if user_input == ‘q’:
break
```

### 실습 예시: 사용자에게 숫자를 입력받아 합계 출력
```
total = 0
i = 1

while i <= 1000:
if i % 3 == 0:
total += i
i += 1

print(total)  # 1부터 1000까지 3의 배수의 합
```

### 실습 예시: 별 출력

```
i = 0
while i < 5:
i += 1
print(’*’ * i)

# 출력

*

**

***

****

*****
```
---

## 반복문 (for 문)

`for` 문은 **시퀀스(리스트, 튜플, 문자열 등)** 안의 요소를 하나씩 꺼내면서 반복 실행한다.

### 기본 구조

```
for 변수 in 시퀀스:
실행할 문장
```

### 예시: 리스트 요소 출력
```
test_list = [‘one’, ‘two’, ‘three’]
for i in test_list:
print(i)
```

### 예시: 튜플을 이용한 for 문
```
a = [(1, 2), (3, 4), (5, 6)]
for (first, last) in a:
print(first + last)
```

### for 문과 range 함수

`range(n)`은 0부터 n-1까지의 숫자 시퀀스를 만든다.
```
for i in range(5):
print(i)  # 0, 1, 2, 3, 4
```
- `range(start, end)` : start부터 end-1까지
- `range(start, end, step)` : step만큼 증가하며 반복
```
for i in range(1, 10, 2):
print(i)  # 1, 3, 5, 7, 9
```

### 예시: 1부터 10까지 합 구하기
```
result = 0
for i in range(1, 11):
result += i

print(result)  # 55
```

### for 문과 len(), range() 함께 사용

리스트의 인덱스로 접근할 때 사용
```
marks = [90, 25, 67, 45, 80]

for i in range(len(marks)):
if marks[i] >= 60:
print(f”{i+1}번 학생은 합격입니다.”)
else:
print(f”{i+1}번 학생은 불합격입니다.”)
```

### continue 문

조건에 맞지 않는 경우 실행을 건너뛴다.
```
marks = [90, 25, 67, 45, 80]

for i in range(len(marks)):
if marks[i] < 60:
continue
print(f”{i+1}번 학생 축하합니다. 합격입니다.”)
```


### for 문과 리스트 컴프리헨션

리스트를 간결하게 생성하는 방법
```
a = [1, 2, 3, 4]
result = [num * 3 for num in a]
print(result)  # [3, 6, 9, 12]
```
조건문도 함께 사용할 수 있다.
```
result = [num * 3 for num in a if num % 2 == 0]
print(result)  # [6, 12]
```
