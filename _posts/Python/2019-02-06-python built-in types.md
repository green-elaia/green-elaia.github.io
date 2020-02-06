---
layout: post
title: python built-in types (numeric, sequence, set, mapping)
category: Python
use_math: true
tag: [int, float, complex, list, tuple, set, dictionary]
show_sidebar: false
---



## Numeric Types

파이썬 numeric types에는 정수(int), 부동소수점(float), 복소수(complex)가 있다.

int 타입은 4바이트, float 타입은 8바이트 (cf. C, Java의 float는 4바이트), complex 타입은 실수부, 허수부 각각 8바이트의 크기를 갖는다.

<br/>

int, float, complex는 모두 불변형(immutable)이다.

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

int(문자열, 밑): 어떤 문자열을 10진법 정수로 변환시키거나 다른 진법의 문자열을 10진법 정수로 변환시키는 함수. '밑'의 범위를 벗어나는 '문자열'을 입력할 경우 ValueError 예외가 발생한다.

```python
>>> a = int('123')
>>> a
123
>>> type(a)
<class 'int'>

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
<class 'float'>
>>> type(float('123'))
<class 'float'>

>>> type(complex(12))
<class 'complex'>
>>> type(complex(12.3))
<class 'complex'>
>>> type(complex('12+3j'))
<class 'complex'>
```

<br/>

부동소수점끼리의 비교는 조심해야 한다.

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

이러한 문제를 해결하기 위해 특정 정밀도까지만을 비교하는 방식을 취할 수 있는데, unittest 모듈의 `assertNotAlmostEqual`(*first*, *second*, *places=7*, *msg=None*, *delta=None*) 함수 등이 있다. 

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

정수와 부동소수점 관련 함수들

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

복소수 관련 함수들

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

