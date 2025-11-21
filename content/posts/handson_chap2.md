---
date: '2025-07-14'
draft: false
title: '핸즈온 머신러닝 - 2장'
tags: ['ML']
summary: 데이터 분석 파이프라인을 알아봅시다.
math: true
---
>2장에서는 캘리포니아 주택 가격 데이터셋을 활용해 머신러닝 프로젝트를 처음부터 끝까지 직접 진행해보면서 배우는 것을 목표로 한다.

## 큰 그림 보기

### 문제 정의

풀고자 하는 문제가 무엇인지 먼저 정의하는 것이 필요함. 그렇게 하여 문제 상황을 정확히 파악하고, 해당 문제에 적합한 시스템을 설계하게 됨. 
지금 문제는 레이블된 훈련 샘플이 있고, 여러 특성을 통해 한 개의 값을 예측하므로 **지도 학습, 단변량 회귀** 문제가 됨.

### 성능 측정 지표 선택

회귀 문제에서는 일반적으로 평균 제곱근 오차, `RMSE`를 사용한다. 
$RMSE(X, h) = \sqrt{\frac{1}{m} \sum_{i=1}^{n} (h(x^{(i)}) - y^{(i)})^2}$

하지만, **이상치로 보이는 구역이 많은 경우**, 평균 절대 오차, `MAE`를 사용하기도 한다.
$MAE(X, h) = \frac{1}{m}\sum_{i=1}^{m}|h(x^{(i)})-y^{(i)}|$

둘 모두 예측값의 벡터와 타깃값의 벡터 사이의 거리를 재는 방식이다. 거리 측정에는 여러 방식을 사용할 수 있다.

- **유클리드 노름** : `RMSE` 계산과 같다. $l_2$ 노름이라고도 부른다.
- **맨해튼 노름** : 절댓값의 합을 계산한다. $l_1$ 노름이라고도 부른다.
- $l_k$ 노름 = $(|v_0|^k + |v_1|^k + ... + |v_n|^k)^{\frac{1}{k}}$

### 가정 검사

시스템에서 요구로 출력을 하고 있는 출력이 현재 개발하고자 하는 모델의 출력과 일치한지 (ex: 가정에서는 수치형을 출력하고자 하는데 범주형을 요구한다던가) 확인해야한다.

## 데이터 가져오기


### 데이터 다운로드 
아래 코드를 실행하면 데이터를 다운로드 할 수 있다.

```python
from pathlib import Path
import pandas as pd
import tarfile
import urllib.request

def load_housing_data():
    tarball_path = Path("datasets/housing.tgz")
    if not tarball_path.is_file():
        Path("datasets").mkdir(parents=True, exist_ok=True)
        url = "https://github.com/ageron/data/raw/main/housing.tgz"
        urllib.request.urlretrieve(url, tarball_path)
        with tarfile.open(tarball_path) as housing_tarball:
            housing_tarball.extractall(path="datasets")
    return pd.read_csv("datasets/housing/housing.csv")

housing = load_housing_data()
```

1. `tarball_path`에 있는 파일이 있는지 확인한다.
1-1. 만약 파일이 없다면, `datasets` 폴더를 만들고 그 폴더에 `tgz` 파일을 다운로드받은 뒤, 압축 해제를 한다.
2. 파일을 pandas DataFrame으로 읽어온 결과를 리턴한다.

### 데이터 구조 훑어보기

`head()` 메서드를 사용하면 데이터의 첫 다섯 행을 읽어올 수 있다.
![](https://velog.velcdn.com/images/chae-jpg/post/c1c752c8-baad-4353-b685-d6ce992c5b6c/image.png)

`info()` 메서드는 데이터에 관한 간략한 설명을 보여준다.
특히, 전체 행 수, 각 특성의 데이터 타입, 널이 아닌 값의 개수를 확인하는 데 유용하다.
![](https://velog.velcdn.com/images/chae-jpg/post/69e622c6-e4ad-455c-bb34-8bca5374f218/image.png)

다음과 같은 정보를 알 수 있다.

- 전체 샘플 개수는 20640개이다.
- `total_bedrooms`는 20433개만 널값이 아니다. 즉, 나머지 207개는 이 특성을 가지고 있지 않다.
- `ocean_proximity` 필드를 제외한 모든 특성이 숫자형이다.

---

`value_counts()` 메서드는 어떤 카테고리가 있고, 각 카테고리마다 얼마나 많은 구역이 있는지 확인할 수 있다.
![](https://velog.velcdn.com/images/chae-jpg/post/d765883d-39d3-44a6-a553-9916172afc19/image.png)

`describe` 메서드는 숫자형 특성의 요약 정보를 보여준다.

![](https://i.imgur.com/XAR1pUi.png)

`hist()` 메서드는 모든 숫자형 형태에 대한 히스토그램을 출력할 수 있다.
![](https://i.imgur.com/r711ouE.png)

이를 통해 알 수 있는 사실은 다음과 같다. 
- 중간 소득이 US 달러로 표시되어 있지 않다. 따라서, 그 단위를 통일하는 것이 필요하다.
- 중간 주택 연도와 중간 주택 가격 그래프의 오른쪽 값이 심하게 높아지면서 그래프가 끝나는 것으로 보아 최댓값과 최솟값이 한정되어 있음을 알 수 있다.
- 특성들의 스케일이 많이 다르다.
- 많은 히스토그램에서 오른쪽 꼬리가 더 길다. 이러한 형태는 일부 머신러닝 알고리즘에서 패턴을 찾기 어렵게 만든다.

### 테스트 세트 만들기

- **데이터 스누핑** 편향 : 테스트 세트로 일반화 오차를 추정하여 매우 낙관적인 추정이 나오게 되는 것

다음과 같은 코드로 랜덤으로 테스트 세트를 만들 수 있다.

```python
import numpy as np

def shuffle_and_split_data(data, test_ratio):
    shuffled_indices = np.random.permutation(len(data)) # 괄호 안 숫자 범위로 생성된 배열을 무작위로 섞음.
    test_set_size = int(len(data) * test_ratio)
    test_indices = shuffled_indices[:test_set_size]
    train_indices = shuffled_indices[test_set_size:]
    return data.iloc[train_indices], data.iloc[test_indices]
```

하지만, **프로그램을 실행할 때마다 다른 테스트 세트가 생성된다**는 문제점이 존재한다. 이를 해결하기 위해서는,
1. 처음 실행에서 테스트 세트를 저장하고 다음번 실행에서 불러들이기
2. 항상 같은 난수 인덱스가 생성되도록 `np.random.permutation()`을 호출하기 전에 난수 발생기의 초깃값을 지정하기

하지만 위 방법 또한 데이터셋이 업데이트된 경우 문제가 생긴다. 따라서, 샘플의 식별자를 사용하여 테스트 세트로 보낼지 말지를 결정할 수 있다.

```python
from zlib import crc32

def is_id_in_test_set(identifier, test_ratio):
    return crc32(np.int64(identifier).tobytes()) < test_ratio * 2 ** 32

def split_data_with_id_hash(data, test_ratio, id_column):
    ids = data[id_column]
    in_test_set = ids.apply(lambda id_: is_id_in_test_set(id_, test_ratio))
    return data.loc[~in_test_set], data.loc[in_test_set]
```

위 방법을 통해 나눈 훈련 / 테스트 세트를 보면 다음과 같다.

![](https://i.imgur.com/gTLpeMA.png)

---
사이킷런을 사용하면 위 과정을 간단히 구현할 수 있다. 가장 간단한 함수는 `train_test_split`으로, *난수 초깃값을 설정하는 기능*과 *행의 개수가 같은 여러 개의 데이터셋을 넘겨 동일한 인덱스를 기반으로 나누는 기능*이 존재한다. 

```python
from sklearn.model_selection import train_test_split

train_set, test_set = train_test_split(housing, test_size=0.2, random_state=42)
```

---
앞서 본 샘플링은 **순수한 랜덤 샘플링**이다. 데이터셋이 충분히 크다면 괜찮지만, 그렇지 않다면 샘플링 편향이 생길 가능성이 크다. 이를 위해 **계층적 샘플링**이 존재한다.

- **계층적 샘플링** : 전체를 *계층*이라는 동질의 그룹으로 나누고, 테스트 세트가 전체를 대표할 수 있도록 각 계층에서 올바른 수의 샘플을 추출하는 것.

이를 위해 전체를 여러 개의 계층으로 나눈 뒤, 계층의 비율대로 전체에서 샘플링하는 것이 가능하다.

```python
housing["income_cat"] = pd.cut(housing["median_income"],
    bins=[0., 1.5, 3.0, 4.5, 6., np.inf],
	labels=[1, 2, 3, 4, 5])
```

[pandas.cut reference](https://pandas.pydata.org/docs/reference/api/pandas.cut.html)
`pandas.cut` 함수는 특정 1차원 배열을 `bins`에 따라 해당 개수, 또는 해당 구간에 따라 분할한 뒤, 각 값들에 대해 해당하는 구간의 `labels`의 값을 부여한다.
사이킷런의 `sklearn.model_selection` 패키지 안에는 여러 가지 분할기 클래스를 제공한다. 모든 분할기는 또한 훈련과 테스트 분할에 대한 반복자를 반환하는 `split()` 메소드를 가지고 있다.

이 코드에서는 `StratifiedShuffleSplit`을 사용해 10개의 다른 계층 분할을 생성한다.

```python
from sklearn.model_selection import StratifiedShuffleSplit

splitter = StratifiedShuffleSplit(n_splits=10, test_size=0.2, random_state=42)
strat_splits = []
for train_index, test_index in splitter.split(housing, housing["income_cat"]):
	strat_train_set_n = housing.iloc[train_index]
	strat_test_set_n = housing.iloc[test_index]
	strat_splits.append([strat_train_set_n, strat_test_set_n])
```

첫 번째 분할을 다음과 같이 사용할 수 있다.

```python
strat_train_set, strat_test_set = strat_splits[0]
```

다음과 같은 방식으로 계층적 샘플링을 코드 한 줄로 불러오는 것도 가능하다.

```python
strat_train_set, strat_test_set = train_test_split(housing, test_size=0.2, stratify=housing["income_cat"], random_state=42)
```

### 데이터 이해를 위한 탐색과 시각화

먼저, 다음과 같은 코드를 통해 지리 정보를 산점도로 만들어 시각화해보자.

```python
housing.plot(kind="scatter", x="longitude", y="latitude", grid=True)
plt.xlabel("경도")
plt.ylabel("위도")
plt.show()
```

![](https://i.imgur.com/68dJHTL.png)

이 그래프에서, 데이터 포인트가 밀접된 영역을 확인해보자.

```python
housing.plot(kind="scatter", x="longitude", y="latitude", grid=True, alpha=0.2)
plt.xlabel("logitude")
plt.ylabel("latitude")
plt.show()
```

![](https://i.imgur.com/SlD9new.png)

다음으로는 구역의 인구와 주택가격까지 그림에 나타내보자.

```python
housing.plot(kind="scatter", x="longitude", y="latitude", grid=True,
			 s=housing["population"] / 100, label="population",
			 c="median_house_value", cmap="jet", colorbar=True,
			 legend=True, figsize=(10, 7))
plt.show()
```

![](https://i.imgur.com/Czh9CEq.png)

### 상관관계 조사하기

모든 특성 간의 **표준 상관계수**를 `corr()` 메서드를 사용해 쉽게 계산할 수 있다.

```python
corr_matrix["median_house_value"].sort_values(ascending=False)
```

```text
median_house_value    1.000000
median_income         0.688380
total_rooms           0.137455
housing_median_age    0.102175
households            0.071426
total_bedrooms        0.054635
population           -0.020153
longitude            -0.050859
latitude             -0.139584
Name: median_house_value, dtype: float64
```

상관관계의 범위는 -1부터 1까지로, 1에 가까우면 강한 양의 상관관계, -1에 가까우면 강한 음의 상관관계를 가진다는 뜻이다.

판다스의 `scatter_matrix` 함수를 통해 숫자형 특성 간 산점도를 그려보는 것 또한 가능하다.

```python
from pandas.plotting import scatter_matrix  
  
attributes = ["median_house_value", "median_income", "total_rooms",   
"housing_median_age"]  
scatter_matrix(housing[attributes], figsize=(12, 8))  
plt.show()
```

![](https://i.imgur.com/Ou17vi8.png)

이 그림을 보았을 때, 중간 주택 가격을 예측하는 데 중간 소득이 가장 유용해보인다는 것을 발견할 수 있다. 이 산점도를 확대해보자.

![](https://i.imgur.com/srlcADH.png)

이 그래프를 통해 알 수 있는 사실은 다음과 같다.

1. 상관관계가 매우 강하다.
	- 위쪽으로 향하는 경향이 보인다.
	- 포인트들이 너무 많이 퍼져 있지는 않다.
2.  가격의 한계값이 수평선으로 잘 보이는데, 이러한 형태가 다른 곳에서도 더 나타나므로 이상한 형태를 알고리즘이 학습하지 않도록 구역을 제거할 수 있다.

### 특성 조합으로 실험하기

특정 구역의 가구 당 방 개수가 몇 개인지, 방 당 침실 개수가 몇 개인지 등 여러 특성을 조합하여 새로운 특성을 만들 수 있다.

```python
housing["rooms_per_house"] = housing["total_rooms"] / housing["population"]  
housing["bedrooms_ratio"] = housing["total_bedrooms"] / housing["total_rooms"]  
housing["population_per_house"] = housing["population"] / housing["households"]
```

```text
median_house_value      1.000000
median_income           0.688380
rooms_per_house         0.202050
total_rooms             0.137455
housing_median_age      0.102175
households              0.071426
total_bedrooms          0.054635
population             -0.020153
population_per_house   -0.038224
longitude              -0.050859
latitude               -0.139584
bedrooms_ratio         -0.256397
Name: median_house_value, dtype: float64
```

기존의 다른 특성들에 비해 `bedrooms_ratio`, `rooms_per_house`  특성이 중간 주택 가격과의 상관관계가 높게 나타나는 것을 확인할 수 있다.

## 머신러닝 알고리즘을 위한 데이터 준비

머신러닝 알고리즘을 위해 데이터를 준비하는 과정은 함수를 만들어 자동화해야 하는데, 그 이유는 다음과 같다.

- 어떤 데이터셋에 대해서도 데이터 변환을 손쉽게 반복할 수 있음.
- 향후 프로젝트에 재사용 가능한 변환 라이브러리를 점진적으로 구축할 수 있음.
- 실제 시스템에서 알고리즘에 새 데이터를 주입하기 전에 이 함수를 사용해 변환할 수 있음.
- 여러 가지 데이터 변환을 쉽게 시도해볼 수 있고 어떤 조합이 가장 좋은지 확인하는 데 편리함.

### 데이터 정제

먼저, `total_bedrooms` 특성에 값이 없는 경우가 있는데, 이를 수정해보자. 방법에는 다음과 같이 세 가지가 있다.

> 1. 해당 구역을 제거하기
> 2. 전체 특성을 삭제하기
> 3. **대체** : 누락된 갓을 어떤 값으로 채우기

판다스의 `dropana()`, `drop()`, `fillna()` 메서드로 이런 작업을 간단하게 처리할 수 있다.

```python
# housing.dropna(subset=["total_bedrooms"], inplace=True)  
  
# housing.drop("total_bedrooms", axis=1, inplace=True)  
  
median = housing["total_bedrooms"].median()  
housing["total_bedrooms"].fillna(median, inplace=True)
```

### 데이터 정제

먼저, `total_bedrooms` 특성에 값이 없는 경우가 있는데, 이를 수정해보자. 방법에는 다음과 같이 세 가지가 있다.

> 1. 해당 구역을 제거하기
> 2. 전체 특성을 삭제하기
> 3. **대체** : 누락된 값을 어떤 값으로 채우기

판다스의 `dropana()`, `drop()`, `fillna()` 메서드로 이런 작업을 간단하게 처리할 수 있다.

```python
# housing.dropna(subset=["total_bedrooms"], inplace=True)  
  
# housing.drop("total_bedrooms", axis=1, inplace=True)  
  
median = housing["total_bedrooms"].median()  
housing["total_bedrooms"].fillna(median, inplace=True)
```

여기까지는 저번 파트에서 다루었던 내용이다.

---

세 번째 옵션이 누락된 값을 중간값으로 채우는 코드이다.
위와 같은 동작을 사이킷런의 `SimpleImputer` 클래스를 통해 할 수 있다. 이 클래스는 각 특성의 중간값을 저장하고 있어 유용하다. 또한, 훈련 세트뿐만 아니라 검증 세트와 테스트 세트 그리고 모델에 주입될 새로운 데이터에 있는 누락된 값을 대체할 수 있다.

다음과 같이 누락된 값을 특성의 중간값으로 대체하도록 지정하여 SimpleImputer의 객체를 생성한다.

```python
from sklearn.impute import SimpleImputer  
  
imputer = SimpleImputer(strategy="median")
```

중간값은 수치형 특성에서만 계산될 수 있으므로 수치 특성만 가진 데이터 복사본을 생성한다.

```python
housing_num = housing.select_dtypes(include=[np.number])
```

이후, imputer 객체의 `fit()` 메서드를 사용해 훈련 데이터에 적용할 수 있다.

```python
imputer.fit(housing_num)
```

imputer은 각 특성의 중간값을 계산하여 그 결과를 객체의 `statistics_` 속성에 저장한다. 
이후, 학습된 impuer 객체를 사용해 훈련 세트에서 누락된 값을 학습된 중간값으로 바꿀 수 있다.

```python
X = imputer.transform(housing_num)
```

누락된 값을 평균이나 가장 자주 등장하는 값, 상수로 바꾸는 것도 가능하다. 뒤 두 가지 방법은 수치가 아닌 데이터도 지원한다.

사이킷런 변환기는 판다스 데이터프레임이 입력되더라도 넘파이 배열이나 사이파이 희소 행렬을 출력한다. 따라서 `imputer.transform(housing_num)` 의 출력 또한 넘파이 배열이다.

```python
type(X)

>> numpy.ndarray
```

현재 상태에서는 열 이름도 인덱스도 없기 때문에, 이를 데이터프레임으로 감싸서 열 이름과 인덱스를 복원할 수 있다.

```python
housing_tr = pd.DataFrame(X, columns=housing_num.columns,  
                          index=housing_num.index)
```

### 텍스트와 범주형 특성 다루기

위에서는 수치형 특성만을 다뤘고, 이제는 텍스트 특성을 살펴보자.

```python
housing_cat = housing[["ocean_proximity"]]  
housing_cat.head(8)

      ocean_proximity
13096        NEAR BAY
14973       <1H OCEAN
3785           INLAND
14689          INLAND
20507      NEAR OCEAN
1286           INLAND
18078       <1H OCEAN
4396         NEAR BAY
```

```python
housing_cat.value_counts()

ocean_proximity
<1H OCEAN          7274
INLAND             5301
NEAR OCEAN         2089
NEAR BAY           1846
ISLAND                2
Name: count, dtype: int64
```

이를 통해, 이 특성은 범주형 특성임을 알 수 있다. 

이 카테고리를 텍스트에서 숫자로 변환하기 위해 사이킷런의 `OrdinalEncoder`을 사용한다.

```python
from sklearn.preprocessing import OrdinalEncoder  
  
ordinal_encoder = OrdinalEncoder()  
housing_cat_encoded = ordinal_encoder.fit_transform(housing_cat)
```

인코딩된 몇 개의 값을 확인하면 다음과 같다.

```python
housing_cat_encoded[:8]

array([[3.],
       [0.],
       [1.],
       [1.],
       [4.],
       [1.],
       [0.],
       [3.]])
```

`categories_` 인스턴스 변수를 사용해 카테고리 리스트를 얻을 수 있다. 범주형 특성마다 1D 카테고리 배열을 담은 리스트가 반환된다.

```python
ordinal_encoder.categories_

[array(['<1H OCEAN', 'INLAND', 'ISLAND', 'NEAR BAY', 'NEAR OCEAN'],
       dtype=object)]
```

이런 표현 방식의 문제는 머신러닝 알고리즘이 가까이 있는 두 값을 떨어져 있는 두 값보다 더 비슷하다고 생각한다는 점이다. (숫자 상으로 비슷하면, 실제로는 전혀 다른 특징을 가진 값이어도 비슷하게 인식하게 된다.)

이러한 문제는 카테고리별 이진 특성을 만들어 해결한다. 카테고리가 `<1H OCEAN`일 때 한 특성이 1이고, 카테고리가 `INLAND`일 때 다른 한 특성이 1이 되는 식이다.
한 특성만 1이고 나머지는 0이므로 이를 **원-핫 인코딩**이라고 부른다. 새로운 특성을 **더미** 특성이라고도 부른다. 사이킷런은 범주 값을 원-핫 벡터로 바꾸기 위한 `OneHotEncoder` 클래스를 제공한다.

```python
from sklearn.preprocessing import OneHotEncoder  
  
cat_encoder = OneHotEncoder()  
housing_cat_1hot = cat_encoder.fit_transform(housing_cat)
```

`OneHotEncoder`의 출력은 넘파이 배열이 아니라 사이파이 희소 행렬이다. 

```python
housing_cat_1hot

<16512x5 sparse matrix of type '<class 'numpy.float64'>'
	with 16512 stored elements in Compressed Sparse Row format>
```

희소 행렬은 0이 대부분인 행렬을 매우 효율적으로 표현한다. 내부적으로 0이 아닌 값과 그 위치만 저장한다. 이는 많은 메모리를 절약하고 계산 속도를 높여주는 효과를 가진다. [공식 documentation](https://docs.scipy.org/doc/scipy/tutorial/sparse.html)

대부분 희소 행렬을 보통의 2D 배열처럼 사용할 수 있지만, 넘파이 배열로 바꾸려면 `toarray()` 메서드를 호출해야 한다.

```python
housing_cat_1hot.toarray()

array([[0., 0., 0., 1., 0.],
       [1., 0., 0., 0., 0.],
       [0., 1., 0., 0., 0.],
       ...,
       [0., 0., 0., 0., 1.],
       [1., 0., 0., 0., 0.],
       [0., 0., 0., 0., 1.]])
```

