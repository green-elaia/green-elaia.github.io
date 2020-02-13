---
layout: post
title: python built-in types (set, mapping)
category: Python
use_math: true
tag: [set, dictionary]
show_sidebar: false
---



## Set & Mapping Types

set & mapping types는 시퀀스 데이터 타입과 달리 데이터를 서로 연관시키지 않고 모아두는 컨테이너(container)이다.

set & mapping types는 다음의 속성을 갖는다.

- 멤버십(membership) 연산 가능: in 키워드 사용
- 크기(size) 함수 사용가능: len(object)
- 반복(iteration) 가능: 반복문에 있는 데이터를 순회할 수 있음

파이썬의 built-in set types에는 set과 frozen set이 있고, built-in mapping types에는 dictionary가 있다.

<br/>

<br/>

##### 집합(set)

set은 반복가능(iterable) 하고, 가변(mutable) 타입이며, 중복 요소가 없는 정렬되지 않은 데이터 타입이다. 인덱스 연산은 불가하고, 멤버십 테스트 및 중복 항목 제거에 사용된다.







##### frozen set





셋과 딕셔너리는 copy()로 깊은 복사를 할 수 있음.



딕셔너리의 언패킹

**언패킹(unpacking)**은 여러개의 객체를 갖고 있는 하나의 객체를 여러개로 나누어 주는 것을 의미한다. 반대로 **패킹(packing)**은 여러개의 객체를 하나의 객체로 묶어주는 것을 의미한다.

keyword가 있는 collection인 딕셔너리를 언패킹 할 수 있는 연산자는 '**'이다. 다음과 같이 딕셔너리를 언패킹하여 함수의 인자로 전달할 수 있다.

```python
>>> a = {"name" : "Park", "age" : 30}
>>> "name: {name}, age: {age}".format(**a)
'name: Park, age: 30'
```

