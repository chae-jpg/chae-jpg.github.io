---
date: '2025-07-10'
draft: false
title: '점프 투 파이썬 - 파이썬 날아오르기'
tags: ['Python']
summary: 파이썬 심화 문법을 알아봅시다.
---

## 클로저와 데코레이터

### 클로저란?

- 함수 안에 내부 함수를 구현하고 그 내부 함수를 리턴하는 함수

ex) 클래스를 통한 구현 (클로저 X)

```python
class Mul:
	def __init__(self, m):
    	self.m = m
    
    def __call__(self, n):
    	return self.m * n
        
if __name__ == "__main__":
	mul3 = Mul(3)
    mul5 = Mul(5)
    
    print(mul3(10)) # 30 출력
    print(mul5(10)) # 50 출력
```

- `mul3 = Mul(3)`에서 생성자를 통해 함수 내부의 m이 3으로 지정.
- `mul3(10)`을 통해 `__call__` 메소드가 호출됨. 

ex) 함수 안에 함수를 구현하는 방법 (클로저)

```
def mul(m):
	def wrapper(n):
    	return m * n
    return wrapper
    
if __name__ == "__main__":
	mul3 = mul(3)
    mul5 = mul(5)
    
    print(mul3(10)) # 30 출력
    print(mul5(10)) # 50 출력
```
- 위와 같은 기능을 구현하고 있지만, **함수가 함수를 리턴**하는 식으로 구현.

### 데코레이터란?

- 기존 함수를 바꾸지 않고 기능을 추가할 수 있게 만드는 클로저

ex)

```python
import time

def elasped(original_func):
	def wrapper():
    	start = time.time()
        result = original_func()
        end = time.time()
        print("함수 수행시간: %f 초" % (end - start))
        return result
    return wrapper
    
def myfunc():
	print("함수가 실행됩니다.")
    
decorated_myfunc = elasped(myfunc)
decorated_myfunc()
```
> **나의 의문** : 여기서 굳이 wrapper을 써서 클로저로 구현해야 하는 이유는? 사실 wrapper 아래 코드를 바로 함수에서 실행해도 작동은 동일할 텐데?

> **이유** : `elasped`를 **데코레이터**로 만들기 위해서. 아래의 데코레이터로써의 기능을 사용하기 위해서는 *클로저로 구현*해야 할 필요가 있음.

- 다음과 같이 @ 문자를 이용해 함수 위에 적용하여 사용할 수 있음.

```python
@elasped
def myfunc():
	print("함수가 실행됩니다.")
    
myfunc() # 데코레이터가 위에 바로 붙었으므로 별도의 선언이 더 이상 필요하지 않음.
```

- 데코레이터와 기존 함수의 인자 개수가 일치하지 않을 경우 에러가 남
-> 어떤 인자를 받든 작동하도록 `*args`와 `**kwargs`를 사용.

```python
def elasped(original_func):
	def wrapper(*args, **kwargs): 
    ...
```

> **나의 의문** : 꼭 `*args`랑 `**kwargs`를 둘 다 써야 하나?

> **해답** : 꼭 그럴 필요는 없는데, 지금처럼 유연하게 어떤 인자가 들어와도 받을 수 있게 처리하는 것이 목적이라면 둘 다 써야 함. 지금의 경우에는 `*args`가 빠질 경우에는 문제가 생김. 위치 인자가 `original_func`로 들어오고 있고, `*args`가 그걸 받는 역할을 하는 인자이기 때문.

## 이터레이터와 제너레이터

### 이터레이터란?

- `이터러블` : 리스트와 같이 반복 구문에 적용할 수 잇는 객체
- `이터레이터` : `next` 함수 호출 시 계속 그다음 값을 리턴하는 객체

ex) 리스트를 이터레이터로 변환하고 next를 사용해 값 가져오기

```python
a = [1, 2, 3]
ia = iter(a) # 모든 이터러블은 iter 함수를 통해 이터레이터로 변환 가능.

next(ia)
# 1
next(ia)
# 2
next(ia)
# 3
next(ia)
# StopIteration 예외 : 더 리턴할 값이 없으므로
```

ex) for을 사용해 이터레이터의 값 가져오기

```python
a = [1, 2, 3]
ia = iter(ia)
for i in ia:
	print(i)
    # 1 2 3 출력
for i in ia:
	print(i)
    # 아무것도 출력 X, for이나 next로 값을 한 번 읽으면 다시 읽을 수 없는 이터레이터의 특징.
```

### 이터레이터 만들기

- class로 이터레이터를 만들 수 있음.
- `__iter__`과 `__next__` 라는 2개의 메서드를 구현.

```python
class MyIterator:
	def __init__(self, data):
    	self.data = data
        self.position = 0
        
    def __iter(self):
    	return self
        
    def __next__(self):
    	if self.position >= len(self.data):
        	raise StopIteration
        result = self.data[self.position]
        self.position += 1
        return result
        
if __name__ == "__main__":
	i = MyIterator([1, 2, 3])
    for item in i:
    	print(item)
```

- `__iter__` : 이 메서드가 있는 클래스로 생성한 객체는 **반복 가능한 객체**가 됨. 반복 가능한 객체를 리턴해야 함.
- `__next__` : `__iter__`이 있는 클래스는 반드시 **`__next__`를 구현**해야 함. 반복 가능한 객체의 값을 차례대로 반환하는 역할을 함.

### 제너레이터란?

- 이터레이터를 생성해 주는 함수
- `next` 함수 호출 시 그 값을 차례대로 얻을 수 있음.
- 차례로 결과를 반환하기 위해 `yield` 키워드 사용

```python
def mygen():
	yield 'a'
    yield 'b'
    yield 'c'

if __name__ == "__main__" : 
    g = mygen()
    next(g)
    # a
    next(g)
    # b
    next(g)
    # c
    next(g)
    # StopIteration 예외
```
- `yield`를 만날 시 해당 값을 리턴하되, 현제 상태를 그대로 기억한 채로 멈춤.

### 제너레이터 표현식

```python
def mygen():
	for i in range(1, 1000):
    	result = i * i
        yield result
        
gen = mygen()

print(next(gen))
# 1
print(next(gen))
# 4
print(next(gen))
# 9
```

다음과 같은 제너레이터를 아래와 같이 튜플 표현식으로 간단하게 만들 수 있음.

```python
gen = (i * i for i in range(1, 1000))
```

### 제너레이터 활용하기

```python
import time

def longtime_job():
    print("job start")
    time.sleep(1)  # 1초 지연
    return "done"
```
다음과 같은 코드에서

`list_job = [longtime_job() for i in range(5)]`와
`list_job = (longtime_job() for i in range(5))`를 사용 가능.

위 : `longtime_job()`을 5번 실행해서 그 결과로 리스트를 만듦. -> 즉시 `longtime_job()` 5번 실행
아래 : 제너레이터 표현식. 즉 `next`로 `list_job`의 결과를 불러오면 그 때마다 실행되고 값을 리스트에 넣음. -> next할 때마다 `longtime_job()` 실행

## 파이썬 타입 어노테이션

### 동적 언어

- `동적 언어` : 프로그램 실행 중 변수의 타입을 바꿀 수 있는 언어. ex) 파이썬

```python
a = 1
a = "1" # 타입을 실행 중 스트링으로 바꿈 -> 동적 언어
```

- 장점 : 유연한 코딩이 가능하므로 쉽고 빠른 개발 가능, 타입 체크를 위한 코드가 없으므로 깔끔한 소스 생성 가능
- 단점 : 프로젝트의 규모가 커질수록 타입을 잘못 사용해 버그가 생길 확률이 늘어남.

### 파이썬 타입 어노테이션

- 3.5버전부터 타입 어노테이션 기능이 도입됨.
- 타입에 대한 힌트를 알려주는 정도의 기능

```python
# a와 b의 타입 : int, 리턴값의 타입 : int
def add(a: int, b: int) -> int:
	return a+b
```

### mypy

- 더 적극적으로 파이썬 어노테이션을 사용할 수 있도록 돕는 라이브러리
- `pip install mypy`를 통해 설치 후 사용
- 아래와 같이 터미널에서 실행 시 결과를 출력함. 

```
C:\doit>mypy typing_sample.py
typing_sample.py:5: error: Argument 2 to "add" has incompatible type "float"; expected "int"
Found 1 error in 1 file (checked 1 source file)
```