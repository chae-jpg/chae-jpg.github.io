---
date: '2025-07-08'
draft: false
title: '점프 투 파이썬 - 파이썬 날개 달기'
tags: ['Python']
summary: 파이썬의 다양한 기능을 알아봅시다.
---

## 클래스 (Class)

클래스는 **자료형을 직접 만들 수 있는 도구**다.  
파이썬은 기본적으로 `int`, `str` 같은 자료형을 제공하지만, 사용자가 직접 자료형(클래스)을 정의할 수 있다.

---

### 클래스 정의하기

```python
class 클래스이름:
def init(self):
# 생성자 (객체가 생성될 때 자동 실행됨)
수행할 문장
```
예시:
```python
class FourCal:
def init(self, first, second):
self.first = first
self.second = second
```
---

### 객체 생성하기
```python
a = FourCal(4, 2)
```
---

### 클래스 내부 함수 (메서드) 정의
```python
class FourCal:
def init(self, first, second):
self.first = first
self.second = second

def add(self):
    return self.first + self.second

def mul(self):
    return self.first * self.second

def sub(self):
    return self.first - self.second

def div(self):
    return self.first / self.second
```

```python
a = FourCal(4, 2)
a.add()  # 6
a.mul()  # 8
a.sub()  # 2
a.div()  # 2.0
```
---

### 생성자 (`__init__`) 이해하기

객체가 생성될 때 자동 호출되어 초기값을 설정함.
```python
class Sample:
def init(self, value):
self.value = value
```
---

### self의 의미

- 클래스 내 메서드는 **반드시 첫 번째 인자로 `self`를 가짐**
- `self`는 생성된 객체 자기 자신을 의미함
- 객체 내 변수 접근: `self.변수`

---

### 클래스에 함수 추가하기
```python
class MoreFourCal(FourCal):
def pow(self):
return self.first ** self.second



a = MoreFourCal(2, 3)
a.pow()  # 8
```
---

### 상속

기존 클래스를 상속받아 새로운 클래스를 만들 수 있다.
```python		
class MoreFourCal(FourCal):
pass  # 기존 기능을 그대로 상속
```
---

### 메서드 오버라이딩 (재정의)

부모 클래스의 메서드를 **자식 클래스에서 다시 정의**할 수 있다.
```python		
class SafeFourCal(FourCal):
def div(self):
if self.second == 0:
return 0
else:
return self.first / self.second



a = SafeFourCal(4, 0)
a.div()  # 0
```
---

### 클래스 변수와 인스턴스 변수

- **클래스 변수**: 클래스에 공유되는 변수
- **인스턴스 변수**: 객체마다 따로 존재
```python
class Family:
lastname = “김”

print(Family.lastname)  # 김

a = Family()
b = Family()
print(a.lastname)  # 김
print(b.lastname)  # 김

Family.lastname = “박”
print(a.lastname)  # 박
print(b.lastname)  # 박
```
※ 클래스 변수는 클래스 자체 또는 모든 인스턴스에서 공유됨

## 모듈 (Module)

모듈이란 **함수, 변수, 클래스** 등을 모아 놓은 **파이썬 파일(.py)** 을 말한다.  
필요할 때 import하여 사용할 수 있다.

---
### 모듈 만들기

예: `mod1.py` 파일을 만든다.
```python
def add(a, b):
return a + b

def sub(a, b):
return a - b
```
---

### 모듈 불러오기 (import)
```python
import mod1
print(mod1.add(3, 4))  # 7
print(mod1.sub(4, 2))  # 2
```
또는 `from`을 사용해 직접 함수만 가져올 수 있다.
```python
from mod1 import add
add(3, 4)  # 7
```
여러 함수 가져오기:

`from mod1 import add, sub`

전체 가져오기:

`from mod1 import *  # 권장되지 않음`

---

### 모듈 이름 확인하기

모듈 내부에서 자신의 이름을 확인할 수 있다.

`print(name)`

직접 실행하면 `__main__`, import되면 파일명이 출력된다.

이를 활용해 테스트 코드를 작성할 수 있다:
```python
def add(a, b):
return a + b

if name == “main”:
print(add(3, 4))
```
---

### 클래스가 포함된 모듈

예: `mod2.py`
```python
class Calculator:
def init(self):
self.result = 0

def add(self, num):
    self.result += num
    return self.result
```
사용 예:
```python
from mod2 import Calculator

cal1 = Calculator()
print(cal1.add(3))  # 3
print(cal1.add(4))  # 7
```
---

### 다른 위치에 있는 모듈 사용하기

- `sys.path`에 디렉토리 추가
```python
import sys
sys.path.append(”/사용할/모듈/디렉토리”)
```
- 또는 환경 변수 `PYTHONPATH`에 디렉토리 등록

---

### 표준 모듈 (라이브러리)

파이썬은 다양한 **표준 모듈**을 제공한다.

예:
```python
import math
print(math.pi)         # 3.141592…
print(math.sqrt(16))   # 4.0

import random
print(random.randint(1, 10))  # 1~10 사이 임의의 정수
```
---

### 외부 모듈

- `pip`을 사용해 설치

`pip install 모듈이름`

예:

`pip install requests`

사용 예:
```python
import requests
res = requests.get(“https://example.com”)
print(res.status_code)
```

## 패키지 (Package)

패키지는 **모듈을 계층적으로 관리**할 수 있게 해주는 **디렉터리 구조**다.  
패키지란, **여러 모듈을 담고 있는 폴더**이다.

---

### 패키지 만들기

다음과 같은 디렉터리 구조를 만든다.
```
game/
├── init.py
├── sound/
│   ├── init.py
│   ├── echo.py
├── graphic/
│   ├── init.py
│   ├── render.py
```
- `__init__.py` 파일은 해당 디렉토리가 **패키지임을 알리는 역할**
- 파이썬 3.3 이상에서는 없어도 되지만, 있는 게 좋다

---

### 모듈 불러오기
```python
import game.sound.echo
game.sound.echo.echo_test()
```
또는 하위 모듈을 직접 import 할 수도 있다:
```python
from game.sound import echo
echo.echo_test()
```
또는 함수만 직접 import:
```python
from game.sound.echo import echo_test
echo_test()
```
---

### 상대 경로로 import 하기

`from . import echo` ← 현재 디렉터리 기준  
`from ..sound import echo` ← 부모 디렉터리 기준

> 상대 경로 import는 **모듈이 패키지 내부에서 실행될 때만 사용 가능**  
> 직접 실행하면 `ImportError` 발생할 수 있음

---

### __init__.py 사용

패키지 디렉터리에 `__init__.py` 파일을 만들어  
패키지를 초기화하거나, 외부에 노출할 모듈을 지정할 수 있다.

예:

```python
# game/sound/__init__.py
__all__ = ['echo']
```
이렇게 하면 from game.sound import * 시 echo만 import 된다.

---

예외 사항

import * 문법은 __init__.py에서 __all__ 리스트로 허용된 것만 가져옴
이 외에는 사용을 권장하지 않음

---

### 패키지와 모듈 위치 설정
- 현재 파이썬이 모듈을 찾는 경로는 sys.path에 저장되어 있음
- 필요하면 경로를 추가해야 함
```python
import sys
sys.path.append("/my/custom/path")
```

## 예외 처리 (Exception Handling)

프로그래밍 중 **오류가 발생할 수 있는 부분**을 처리하여 프로그램이 중단되지 않도록 만드는 방법.

---

### 기본 구조

```python
try:
    실행할 코드
except 예외종류:
    예외 발생 시 실행할 코드
```

예시:

```python
try:
    4 / 0
except ZeroDivisionError:
    print("0으로 나눌 수 없습니다.")
```

---

### 여러 개의 except 문

```python
try:
    a = [1, 2]
    print(a[3])
    4 / 0
except ZeroDivisionError:
    print("0으로 나눌 수 없습니다.")
except IndexError:
    print("인덱스 오류입니다.")
```

→ 가장 먼저 발생한 예외만 처리된다.

---

### 하나의 except에서 여러 예외 처리

```python
try:
    a = [1, 2]
    print(a[3])
except (ZeroDivisionError, IndexError):
    print("오류 발생!")
```

---

### 예외 메시지 확인하기 (`as` 사용)

```python
try:
    4 / 0
except ZeroDivisionError as e:
    print(e)
```

출력: `division by zero`

---

### finally 문

예외 발생 여부와 관계없이 **항상 실행되는 블록**

```python
try:
    print("try 실행")
    1 / 0
except:
    print("except 실행")
finally:
    print("finally 실행")
```

→ 출력:  
```python
try 실행  
except 실행  
finally 실행
```

---

### 예외 발생시키기 (`raise`)

직접 예외를 발생시킬 수 있다.

```python
def raise_error():
    raise NotImplementedError

raise_error()
```

---

### 사용자 정의 예외 만들기

```python
class MyError(Exception):
    pass

def say_nick(nick):
    if nick == "바보":
        raise MyError()
    print(nick)

try:
    say_nick("바보")
except MyError:
    print("사용자 정의 예외 발생!")
```

---

### 예외 메시지 포함하기

```python
class MyError(Exception):
    def __init__(self, msg):
        self.msg = msg
    def __str__(self):
        return self.msg

try:
    raise MyError("오류가 발생했습니다.")
except MyError as e:
    print(e)
```

출력: `오류가 발생했습니다.`

---

### 정리

- `try ~ except`: 예외 발생 시 처리
- `except e`: 예외 메시지 확인
- `finally`: 무조건 실행
- `raise`: 예외 강제 발생
- 사용자 정의 예외: `Exception` 상속

---

## 내장 함수 (Built-in Functions)

파이썬은 기본적으로 다양한 **내장 함수**를 제공한다.  
외부 모듈 없이 언제든지 사용할 수 있다.

---

### abs(x)

절댓값 반환

```python
abs(-3)  # 3
abs(3.14)  # 3.14
```

---

### all(iterable)

모든 요소가 참이면 `True`, 하나라도 거짓이면 `False`

```python
all([1, 2, 3])  # True
all([1, 0, 3])  # False
```

---

### any(iterable)

하나라도 참이면 `True`, 모두 거짓이면 `False`

```python
any([0, "", None])  # False
any([0, "", 1])  # True
```

---

### chr(i)

유니코드 정수 → 문자

```python
chr(97)  # 'a'
```

---

### dir([object])

객체가 가진 **변수나 함수(속성)** 목록 리턴

```python
dir([1, 2, 3])  # list의 메서드 목록 출력
```

---

### divmod(a, b)

`a`를 `b`로 나눈 **몫과 나머지**를 튜플로 반환

```python
divmod(7, 3)  # (2, 1)
```

---

### enumerate(iterable)

반복 가능한 자료형을 (index, 요소) 형태로 반환

```python
for i, name in enumerate(['a', 'b', 'c']):
    print(i, name)
```

---

### eval(expression)

문자열로 된 표현식을 **실행한 결과 반환**

```python
eval("1 + 2")  # 3
eval("'hi' + 'world'")  # 'hiworld'
```

---

### filter(function, iterable)

함수 조건을 만족하는 요소만 필터링

```python
def is_positive(x):
    return x > 0

list(filter(is_positive, [1, -3, 2, 0, -5]))  # [1, 2]
```

---

### hex(x)

정수를 16진수 문자열로 변환

```python
hex(255)  # '0xff'
```

---

### id(object)

객체의 고유 주소값 반환

```python
a = 3
id(a)
```

---

### input([prompt])

사용자 입력 받기 (문자열로 반환됨)

```python
name = input("이름을 입력하세요: ")
```

---

### int(x [, base])

문자열을 정수로 변환 (진수도 지정 가능)

```python
int('3')       # 3
int('11', 2)   # 3
```

---

### isinstance(object, class)

객체가 클래스의 인스턴스인지 검사

```python
isinstance(3, int)  # True
```

---

### len(s)

길이 반환

```python
len("python")  # 6
```

---

### list([iterable])

리스트로 변환

```python
list("abc")  # ['a', 'b', 'c']
```

---

### map(function, iterable)

함수를 반복 가능한 객체의 요소에 적용

```python
def square(x):
    return x * x

list(map(square, [1, 2, 3]))  # [1, 4, 9]
```

---

### max(iterable)

최댓값 반환

```python
max([1, 2, 3])  # 3
```

---

### min(iterable)

최솟값 반환

```python
min([1, 2, 3])  # 1
```

---

### open(filename, mode)

파일을 열어 파일 객체 반환

```python
f = open("file.txt", 'r')
```

---

### ord(c)

문자 → 유니코드 정수

```python
ord('a')  # 97
```

---

### pow(x, y)

`x ** y`와 같음

```python
pow(2, 3)  # 8
```

---

### range([start], stop[, step])

정수 시퀀스 생성

```python
list(range(5))       # [0, 1, 2, 3, 4]
list(range(1, 10, 2))  # [1, 3, 5, 7, 9]
```

---

### round(number[, ndigits])

반올림

```python
round(3.14159, 2)  # 3.14
```

---

### sorted(iterable[, key][, reverse])

정렬된 리스트 반환 (원본 변경 X)

```python
sorted([3, 1, 2])  # [1, 2, 3]
sorted("zero")     # ['e', 'o', 'r', 'z']
```

---

### str(object)

문자열로 변환

```python
str(3)     # '3'
str(3.14)  # '3.14'
```

---

### sum(iterable[, start])

합계 반환

```python
sum([1, 2, 3])  # 6
```

---

### tuple([iterable])

튜플로 변환

```python
tuple([1, 2, 3])  # (1, 2, 3)
```

---

### type(object)

객체의 자료형 반환

```python
type("abc")  # <class 'str'>
```

---

### zip(*iterables)

여러 iterable을 묶어서 튜플의 리스트로 반환

```python
list(zip([1, 2], ['a', 'b']))  # [(1, 'a'), (2, 'b')]
```
---

## 표준 라이브러리

파이썬은 다양한 표준 라이브러리를 제공하며, import만 하면 바로 사용할 수 있다.

---

### sys

시스템 관련 정보를 다루는 모듈

```python
import sys

sys.argv        # 명령행 인수
sys.exit()      # 강제 종료
sys.path        # 모듈 검색 경로
```

예:

```python
# hello.py
import sys
print(sys.argv)
```

명령어:  
`python hello.py arg1 arg2` → `['hello.py', 'arg1', 'arg2']`

---

### pickle

파이썬 객체를 파일에 저장하거나 불러올 때 사용

```python
import pickle

f = open("test.txt", "wb")
data = {1: 'python', 2: 'you'}
pickle.dump(data, f)
f.close()

# 불러오기
f = open("test.txt", "rb")
data = pickle.load(f)
print(data)  # {1: 'python', 2: 'you'}
```

---

### os

운영체제와 상호작용할 수 있는 기능 제공

```python
import os

os.environ        # 환경 변수
os.getcwd()       # 현재 디렉토리
os.chdir("path")  # 디렉토리 이동
os.system("dir")  # 시스템 명령 실행
```

---

### shutil

파일 복사 관련 기능 제공

```python
import shutil

shutil.copy("src.txt", "dst.txt")
```

---

### glob

파일 이름 리스트를 얻을 때 사용 (와일드카드 사용 가능)

```python
import glob

glob.glob("*.py")  # 현재 폴더의 모든 .py 파일 리스트
```

---

### tempfile

임시 파일과 디렉터리를 쉽게 만들 수 있음

```python
import tempfile

f = tempfile.TemporaryFile()
f.write(b"Hello")
f.seek(0)
print(f.read())
f.close()
```

---

### time

시간 관련 함수 제공

```python
import time

time.time()             # 현재 시간 (timestamp)
time.localtime()        # struct_time
time.asctime()          # 보기 좋은 문자열 시간
time.sleep(1)           # 1초 대기
```

예: 1초 간격으로 5번 출력

```python
for i in range(5):
    print(i)
    time.sleep(1)
```

---

### calendar

달력 관련 기능 제공

```python
import calendar

print(calendar.calendar(2025))
print(calendar.month(2025, 7))
calendar.isleap(2024)  # 윤년 여부
```

---

### random

난수 생성 관련 함수 제공

```python
import random

random.random()           # 0~1 사이 임의의 실수
random.randint(1, 10)     # 1~10 사이 정수
random.choice(['a', 'b']) # 리스트에서 임의 선택
random.shuffle([1, 2, 3]) # 리스트 섞기
```

---

### webbrowser

웹 브라우저를 열 수 있음

```python
import webbrowser

webbrowser.open("https://www.google.com")
```

## 외부 라이브러리

파이썬 설치 시 기본으로 제공되지 않는 라이브러리를 외부 라이브러리라고 하며, 사용 전 `pip` 도구로 설치해야 한다.  
`pip`는 ‘**핍**’이라고 읽으며, 의존성 있는 패키지도 함께 설치해준다.

---

### pip 사용법

- 설치:  
  ```python
  pip install SomePackage
  ```

- 제거:  
  ```python
  pip uninstall SomePackage
  ```

- 특정 버전 설치:  
  ```python
  pip install SomePackage==1.0.4
  ```

- 최신 버전 설치:  
  ```python
  pip install --upgrade SomePackage
  ```

- 설치된 패키지 목록 보기:  
  ```python
  pip list
  ```

예시 출력:
```python
Package         Version
--------------- -------
amqp            2.1.4
celery          3.1.0
requests        2.31.0
...
```

---

### Faker

#### 설치

```python
pip install Faker
```

#### 기본 사용

```python
from faker import Faker

fake = Faker()
fake.name()       # 'Matthew Estrada' 등
fake.address()    # '충청북도 수원시 잠실6길 ...'
```

- 한국어 데이터 사용:
  ```python
  fake = Faker('ko-KR')
  fake.name()  # '김하은'
  ```

- 테스트용 데이터 30건 생성:
  ```python
  test_data = [(fake.name(), fake.address()) for i in range(30)]
  ```

#### 주요 메서드

| 메서드 | 설명 |
|--------|------|
| fake.name() | 이름 |
| fake.address() | 주소 |
| fake.postcode() | 우편번호 |
| fake.country() | 국가명 |
| fake.company() | 회사명 |
| fake.job() | 직업명 |
| fake.phone_number() | 전화번호 |
| fake.email() | 이메일 |
| fake.user_name() | 사용자명 |
| fake.pyint(min, max) | 임의의 정수 |
| fake.ipv4_private() | IP 주소 |
| fake.text() | 문장 |
| fake.color_name() | 색상명 |

---

### sympy

수학식 기호화 및 방정식 풀이용 외부 라이브러리

#### 설치

```python
pip install sympy
```

---

#### 예제: 일차방정식 풀이

```python
from fractions import Fraction
import sympy

x = sympy.symbols("x")  # 미지수 x
f = sympy.Eq(x * Fraction('2/5'), 1760)  # x*(2/5) = 1760

result = sympy.solve(f)  # [4400]
remains = result[0] - 1760

print(f"남은 돈은 {remains}원입니다.")  # 2640
```

- `sympy.symbols("x")`: x라는 미지수 기호 생성
- `sympy.Eq(a, b)`: a = b 형태 방정식 표현
- `sympy.solve()`: 방정식의 해 구하기
- `fractions.Fraction`: 유리수 정확히 표현

---

#### 예제: 이차방정식 풀이

```python
import sympy

x = sympy.symbols("x")
f = sympy.Eq(x**2, 1)
sympy.solve(f)  # [-1, 1]
```

---

#### 예제: 연립방정식 풀이

```python
import sympy

x, y = sympy.symbols("x y")
f1 = sympy.Eq(x + y, 10)
f2 = sympy.Eq(x - y, 4)
sympy.solve([f1, f2])  # {x: 7, y: 3}
```

- 미지수가 2개 이상인 경우 `dict` 형태로 반환됨
