---
layout: post
title: python built-in types (numeric)
category: Python
use_math: true
tag: [int, float, complex]
show_sidebar: false
---



## Numeric Types

파이썬 numeric types에는 정수(int), 부동소수점(float), 복소수(complex)가 있다.

int 타입은 4바이트, float 타입은 8바이트 (cf. C, Java의 float는 4바이트), complex 타입은 실수부, 허수부 각각 8바이트의 크기를 갖는다.

<br/>

<br/>

##### int, float, complex는 모두 불변형(immutable)이다.

불변형이라는 것은 데이터가 변경 불가능하다는 것이다. 반대로 가변형(mutable)은 데이터의 변경이 가능하다는 의미이다.

```python
a = 5
a = 3
```

파이썬의 모든 것은 객체이다. 위의 코드의 첫번째 줄은 5라는 값을 가지는 정수타입의 객체를 만들고, 그것의 레퍼런스를 변수 a에 저장한다는 것을 의미한다.

만약, 두번째 줄처럼 변수 a에 3를 할당한다고 하면, 첫줄에서 만들어진 값이 5인 객체의 값이 3으로 변경되는 것이 아니라, 값이 3인 정수타입의 객체가 새로 만들어지고 이것의 레퍼런스가 변수 a에 저장되는 것이다. 아래의 코드를 보면 각각의 경우에서 변수 a가 갖고 있는 레퍼런스 값이 다르다는 것을 알 수 있다.

```python
>>> a = 5
>>> id(a)
140736155325376
>>> a = 3
>>> id(a)
140736155325312
```

<br/>

<br/>

##### int(*문자열*, *밑* ) 함수

: 어떤 문자열을 10진법 정수로 변환시키거나 다른 진법의 문자열을 10진법 정수로 변환시키는 함수. '밑'의 범위를 벗어나는 '문자열'을 입력할 경우 ValueError 예외가 발생한다.

```python
>>> a = int('123')
>>> a
123
>>> type(a)
<class "int">

# 2진법 '100'를 10진법 정수로 변환
>>> int('100',2)
4
# 16진법 'AF'를 10진법 정수로 변환
>>> int('AF',16)
175

# 밑의 범위를 벗어나는 문자열을 입력했을 경우 ValueError 예외 발생
>>> int('12',2)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: invalid literal for int() with base 2: '12'
```

<br/>

정수나 문자열을 부동소수점으로 변환하는 float(),

정수, 실수, 문자열을 복소수로 변환하는 complex() 함수도 존재한다.

```python
>>> type(float(123))
< class 'float' >
>>> type(float('123'))
< class 'float' >

>>> type(complex(12))
< class 'complex' >
>>> type(complex(12.3))
< class 'complex' >
>>> type(complex('12+3j'))
< class 'complex' >
```

<br/>

<br/>

##### 부동소수점끼리의 비교는 조심해야 한다.

부동소수점은 이진수 분수(binary fraction)로 표현된다. 그래서 2진수는 정확히 10진수로 표현이 가능하지만, 10진수는 2진수로 정확히 표현할 수 없는 경우가 발생한다 (10진수 0.1 = 2진수 0.00110011001100...) . 이런 이유 때문에 다음과 같은 결과가 발생한다.

```python
>>> 0.2 *3 == 0.6
False
>>> 1.2 - 0.2 == 1.0
True
>>> 1.2 -0.1 == 1.1
False
>>> 0.1 * 0.1 == 0.01
False
```

이러한 문제를 해결하기 위해 특정 정밀도까지만을 비교하는 방식을 취할 수 있는데, unittest 모듈의 assertNotAlmostEqual(*first*, *second*, *places=7*, *msg=None*, *delta=None*) 함수 등이 있다. 

```python
>>> import unittest
>>> test = unittest.Testcase()

# 두 수가 같으면 아무것도 리턴하지 않지만, 다를 경우 AssertionError를 발생시킴
>>> test.assertAlmostEqual(0.2*3, 0.6)
>>> test.assertAlmostEqual(0.2*3, 0.7)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "C:\ProgramData\Anaconda3\lib\unittest\case.py", line 893, in assertAlmostEqual
    raise self.failureException(msg)
AssertionError: 0.6000000000000001 != 0.7 within 7 places (0.09999999999999987 difference)
```

<br/>

<br/>

##### 정수와 부동소수점 관련 함수들

```python
# '/'는 부동소수점을 리턴, '//'는 몫을 리턴, '%'는 나머지를 리턴함
# divmod(피제수 dividend, 제수 divisor) 함수는 (몫, 나머지)를 리턴함
>>> 7/2
3.5
>>> 7//2
3
>>> 7%2
1
>>> divmod(7,2)
(3, 1)

"""
round(x,n) 함수는
n이 양수이면 반올림하여 소수점 n번째 자리부터 표현한 값을 리턴하고,
n이 음수이면 |n|자리에서 반올림한 값을 리턴함.
n의 default는 0이고 소수점 첫자리에서 반올림한 값을 리턴함.
"""
>>> round(123.1234, 3)
123.123
>>> round(123.1234, -1)
120.0
>>> round(123.1234)
123

# 부동소수점.as_integer_ratio() 함수는 부동소수점을 분수로 표현하여 리턴함
>>> 2.75.as_integer_ratio()
(11, 4)
```

<br/>

##### 복소수 관련 함수들

아래의 함수들 외에 math 모듈에서 제공하는 대부분의 삼각함수, 로그함수의 복소수 버전을 이용하고 싶다면 cmath 모듈을 사용하면 된다.

```python
>>> z = 12 + 34j
>>> z.real  # 실수부
12.0
>>> z.imag  # 허수부
34.0
>>> z.conjugate()  # 켤레복소수
(12-34j)
```

<br/>

<br/>

##### 분수를 다룰 수 있는 fractions 모듈

```python
>>> from fractions import Fraction

>>> a = Fraction(*1.25.as_integer_ratio())  # '*'는 unpacking 연산자
>>> a
Fraction(5, 4)
>>> a.denominator  # 분모
4
>>> a.numerator  # 분자
5
```

<br/>

##### decimal 모듈

정확한 10진법의 부동소수점 숫자가 필요한 경우, 부동소수점 불변 타입인 decimal.Decimal(*문자열 또는 정수*)를 사용할 수 있다. 앞서 살펴본 것과 같이 10진법 0.1은 정확하게 2진법으로 표현할 수가 없다. 하지만 decimal.Decimal 객체를 쓰면 10진법 0.1을 정확하게 2진법으로 표현할 수 있다. 즉, 정확도가 필요한 경우 decimal 모듈을 사용하는 것이 좋다.

```python
>>> sum(0.1 for i in range(10))
0.9999999999999999
>>> sum(0.1 for i in range(10)) == 1.0
False

>>> from decimal import Decimal
>>> sum(Decimal('0.1') for i in range(10)) == Decimal('1.0')
True
```

<br/>

##### 10진수 정수를 2진수, 8진수, 16진수 문자열로 변환하는 함수

```python
>>> bin(20)  # 정수 20을 2진수 문자열로 리턴
'0b10100'
>>> oct(20)  # 정수 20을 8진수로 문자열로 리턴
'0o24'
>>> hex(20)  # 정수 20을 16진수로 문자열로 리턴
'0x14'
```

<br/>

##### 난수를 생성하는 random 모듈

```python
>>> import random

>>> a = [1,2,3,4,5]
>>> random.choice(a)  # 리스트 a에서 임의의 수 하나를 선택
3
>>> random.sample(a,3)  # 리스트 a에서 크기가 3인 임의의 샘플을 선택
[1, 3, 5]
>>> random.shuffle(a)  # 리스트 a의 순서를 섞음
>>> a
[5, 4, 1, 3, 2]
>>> random.randint(0,20)  # 0~20까지의 범위에서 임의의 정수를 하나 생성
14
```

<br/>

##### numpy 패키지

넘파이 패키지는 3rd party package로서 대규모의 다차원 배열 및 행렬을 지원한다. 또한 배열 연산에 쓰이는 수학 함수 라이브러리를 제공한다.

```python
>>> import numpy as np

>>> x = np.array(((11,12,13),(21,22,23),(31,32,33)))  # numpy 배열 생성
>>> x
array([[11, 12, 13],
       [21, 22, 23],
       [31, 32, 33]])
>>> x.ndim  # numpy 배열의 차원수
2
```

넘파이 배열을 생성하면 요소별 연산이 가능하다.

```python
>>> import numpy as np

>>> ax = np.array([1,2,3])
>>> ay = np.array([3,4,5])
>>> ax * 2  # 요소별 곱셈
array([2, 4, 6])
>>> ax + 10  # 요소별 덧셈
array([11, 12, 13])
>>> np.sqrt(ax)  # 요소별 제곱근
array([1.        , 1.41421356, 1.73205081])
>>> np.cos(ax)  # 요소별 cosine
array([ 0.54030231, -0.41614684, -0.9899925 ])
>>> ax-ay  # 요소별 뺄셈
array([-2, -2, -2])
>>> np.where(ax<2, ax, 10)  # where(조건문, 참일 때, 거짓일 때)
array([ 1, 10, 10])
>>> m = np.matrix([ax, ay, ax])  # numpy 행렬 생성
>>> m
matrix([[1, 2, 3],
        [3, 4, 5],
        [1, 2, 3]])
>>> m.T  # 전치행렬
matrix([[1, 3, 1],
        [2, 4, 2],
        [3, 5, 3]])
>>> g1 = np.zeros(shape=(3,4), dtype=float)  # 실수 타입으로 0을 채운 3 by 4 넘파이 배열 생성
>>> g1
array([[0., 0., 0., 0.],
       [0., 0., 0., 0.],
       [0., 0., 0., 0.]])
>>> type(g1)
< class 'numpy.ndarray' >
>>> g2 = np.ones(shape=(3,4), dtype=float)  # 실수 타입으로 1을 채운 3 by 4 넘파이 배열 생성
>>> g2
array([[1., 1., 1., 1.],
       [1., 1., 1., 1.],
       [1., 1., 1., 1.]])
>>> g2[1] += 4  # 넘파이 배열 첫번째 차원의 index 1인 곳에 요소별로 4씩 더함
>>> g2
array([[1., 1., 1., 1.],
       [5., 5., 5., 5.],
       [1., 1., 1., 1.]])
>>> g2[:,1] *= 2  # 넘파이 배열 첫번째 차원의 모든 요소의 두번째 차원의 index가 1인 곳에 2씩 곱함
>>> g2
array([[ 1.,  2.,  1.,  1.],
       [ 5., 10.,  5.,  5.],
       [ 1.,  2.,  1.,  1.]])
```

<br/>

넘파이 배열은 파이썬의 내장 타입인 리스트보다 더 효율적이다.

```python
import numpy as np
import time

def list_version():
    t1 = time.time()
    x = range(10000000)
    y = range(10000000)
    z = []
    for i in range(10000000):
        z.append(x[i]+y[i])
    return time.time() - t1

def numpy_version():
    t1 = time.time()
    x = np.arange(10000000)
    y = np.arange(10000000)
    z = x + y
    return time.time() - t1

if __name__ == "__main__":
    print('list_version: {0} 초'.format(list_version()))
    print('numpy_version: {0} 초'.format(numpy_version()))

"""
실행결과
list_version: 3.119636297225952 초
numpy_version: 0.059209585189819336 초
"""
```

