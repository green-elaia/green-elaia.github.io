---
layout: post
title: python built-in types (set, mapping)
category: Python
use_math: true
tag: [set, dictionary]
show_sidebar: false
---





셋과 딕셔너리는 copy()로 깊은 복사를 할 수 있음.



딕셔너리의 언패킹

**언패킹(unpacking)**은 여러개의 객체를 갖고 있는 하나의 객체를 여러개로 나누어 주는 것을 의미한다. 반대로 **패킹(packing)**은 여러개의 객체를 하나의 객체로 묶어주는 것을 의미한다.

keyword가 있는 collection인 딕셔너리를 언패킹 할 수 있는 연산자는 '**'이다. 다음과 같이 딕셔너리를 언패킹하여 함수의 인자로 전달할 수 있다.

```python
>>> a = {"name" : "Park", "age" : 30}
>>> "name: {name}, age: {age}".format(**a)
'name: Park, age: 30'
```

