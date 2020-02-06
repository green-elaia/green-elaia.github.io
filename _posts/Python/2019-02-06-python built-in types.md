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

<br/>

int, float, complex는 모두 불변형(immutable)이다.

불변형이라는 것은 데이터가 변경 불가능하다는 것이다. 반대로 가변형(mutable)은 데이터의 변경이 가능하다는 의미이다.

```python
a = 5
a = 3
```

파이썬의 모든 것은 객체이다. 위의 코드의 첫번째줄은 5라는 값을 가지는 정수타입의 객체를 만들고, 그것의 레퍼런스를 변수 a에 저장한다는 것을 의미한다.

만약, 두번째줄처럼 변수 a에 3를 저장한다고 한다면, 첫줄에서 만들어진 값이 5인 객체의 값이 3으로 변경되어 