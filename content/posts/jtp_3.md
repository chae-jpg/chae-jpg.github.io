---
date: '2025-07-07'
draft: false
title: '점프 투 파이썬 - 파이썬의 입출력'
tags: ['Python']
summary: 파이썬의 입출력을 알아봅시다.
---

## 함수

### 파이썬 함수의 구조

파이썬 함수의 구조는 다음과 같다.

```python
def 함수_이름(매개변수):
    수행할_문장1
    수행할_문장2
    ...
```

C, Java와 같은 언어처럼 리턴값과 매개변수의 타입을 명시하지 않아도 된다.

### 여러 개의 입력값을 받는 함수 만들기

매개변수 이름 앞에 `*`를 붙이면 입력값을 모두 모아 튜플로 만들어준다.

```python
>>> def add_many(*args): 
...     result = 0 
...     for i in args: 
...         result = result + i   # *args에 입력받은 모든 값을 더한다.
...     return result 

```

*가 붙은 매개변수만 함수의 인자로 사용할 수 있는 것은 아니다. 다음과 같은 형태로도 사용할 수 있다.

```python
>>> def add_mul(choice, *args): 
...     if choice == "add":   # 매개변수 choice에 "add"를 입력받았을 때
...         result = 0 
...         for i in args: 
...             result = result + i 
...     elif choice == "mul":   # 매개변수 choice에 "mul"을 입력받았을 때
...         result = 1 
...         for i in args: 
...             result = result * i 
...     return result 
```

### 키워드 매개변수, kwargs

키워드 매개변수에는 이름 앞에 별 2개를 붙인다.
키워드 매개변수는 함수 내에서 딕셔너리가 되고, `Key=Value` 형태의 입력값이 딕셔너리에 저장된다.

```python
>>> def print_kwargs(**kwargs):
...     print(kwargs)


>>> print_kwargs(a=1)
{'a': 1}
>>> print_kwargs(name='foo', age=3)
{'age': 3, 'name': 'foo'}

```

### 매개변수에 초깃값 미리 설정하기

아래와 같이 매개변수 자체에 초깃값을 설정해 함수 호출 시 값을 전달하지 않고도 기존에 저장된 값을 불러오도록 할 수 있다.
```python
def hello(name, age, man=True):
	print("f'이름 : {name}, 나이 : {age}, 사람 여부 : {man}'")
    
# 둘 모두 같은 결과를 출력
hello("이채원", 24)
hello("이채원", 24, True)
```

### lambda 예약어

def와 같은 역할을 하지만, def를 사용해야 할 정도로 복잡하지 않거나 def를 사용할 수 없는 곳에 사용한다.

다음과 같은 형태를 갖는다.

```python
함수_이름 = lambda 매개변수1, 매개변수2, ... : 매개변수를_이용한_표현식

# 예
>>> add = lambda a, b: a+b
>>> result = add(3, 4)
>>> print(result)
7
```

## 사용자 입출력

### input 사용하기

input 함수를 사용하면 사용자가 키보드로 입력한 모든 것을 문자열로 저장할 수 있다.
프롬프트를 띄우고 싶을 경우에는 원하는 문자열을 매개변수로 넘겨준다.

```python
>>> number = input("숫자를 입력하세요: ")
숫자를 입력하세요:
```

## 파일 읽고 쓰기

### open 함수

아래의 방법으로 open 함수를 사용해 여러 모드로 파일을 열 수 있다.

```python
파일_객체 = open(파일_이름, 파일_열기_모드)
```

파일 열기 모드는 다음과 같다.

|파일 열기 모드|설명|
|-----------|--|
|r|읽기 모드 : 파일을 읽기만 할 때 사용|
|w|쓰기 모드 : 파일에 내용을 쓸 때 사용|
|a|추가 모드 : 파일의 마지막에 새로운 내용을 추가할 때 사용|

### 파일 쓰기

파일을 쓰기 위해서는 `write` 함수를 사용한다. 
예제는 다음과 같다.

```python
# write_data.py
f = open("C:/doit/새파일.txt", 'w')
for i in range(1, 11):
    data = "%d번째 줄입니다.\n" % i
    f.write(data)
f.close()
```
위 예제는 파일을 읽어올 때 `w` 모드로 읽어왔다.
위 방법으로 파일을 읽을 시에는 **기존 파일의 내용이 전부 사라진다.**
하지만 `a` 모드로 불러올 시, 기존 파일의 내용은 유지하면서 뒤에 덧붙여 새로운 내용을 쓸 수 있다.

### 파일을 읽는 여러 가지 방법

#### readline 함수 사용하기

`readline` 함수는 파일의 내용을 **한 줄씩** 읽는 함수이다. 
다음과 같은 방법으로 파일의 모든 내용을 읽고 출력하는 프로그램을 작성할 수 있다.

```python
# readline_all.py
f = open("C:/doit/새파일.txt", 'r')
while True:
    line = f.readline()
    if not line: break
    print(line)
f.close()
```

#### readlines 함수 사용하기

`readlines`는 파일의 **모든 줄**을 읽어 각각의 줄을 요소로 가지는 리스트를 리턴한다.

```python
# readlines.py
f = open("C:/doit/새파일.txt", 'r')
lines = f.readlines()
for line in lines:
    print(line)
f.close()
```

#### read 함수 사용하기

`read`는 파일의 내용 전체를 문자열로 리턴한다.

```python
# read.py
f = open("C:/doit/새파일.txt", 'r')
data = f.read()
print(data)
f.close()
```

#### 파일 객체를 for문과 함께 사용하기

파일 객체와 for을 함께 사용하면 파일을 줄 단위로 읽을 수 있다.

```python
# read_for.py
f = open("C:/doit/새파일.txt", 'r')
for line in f:
    print(line)
f.close()
```

### with문과 함께 사용하기

위의 예제에서는 파일을 연 뒤(`open()`) 닫는 과정(`f.close()`)를 반복했다. 
파일을 닫을 필요 없이 연 뒤 모든 코드가 끝나면 자동으로 닫히게 할 수 있는 방법이 있다. with문을 사용하면 가능하다. 예제는 아래와 같다.

```python
with open("foo.txt", "w") as f:
    f.write("Life is too short, you need python")
```

## 프로그램의 입출력

### sys 모듈 사용하기

`sys` 모듈을 사용하면 프로그램을 실행할 때 인수를 전달하여 해당 인수를 불러오는 것이 가능하다. 

```python
# sys2.py
import sys
args = sys.argv[1:]
for i in args:
    print(i.upper(), end=' ')
```

위 예시는 프롬프트 상에서 프로그램이 실행될 때 받았던 인수를 넘겨받은 뒤 해당 인수를 모두 대문자로 변환하여 한 줄에 출력하는 코드이다.

여기서 `sys.argv[1:]`은 함수 이름을 제외한 모든 인수이다.
![](https://velog.velcdn.com/images/chae-jpg/post/340c4023-2134-414f-999b-09d6cfb10209/image.png)
