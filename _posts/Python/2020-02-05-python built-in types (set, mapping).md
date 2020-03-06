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

set과 dictionary는 가변(mutable) 타입이므로 객체를 복사할 때 주의해야 한다. set과 dictionary는 built-in function인 copy() 함수로 깊은 복사를 할 수 있다.

<br/>

<br/>

##### 집합(set)

set은 반복가능(iterable) 하고, 가변(mutable) 타입이며, 중복 요소가 없는 정렬되지 않은 데이터 타입이다. 인덱스 연산은 불가하고, 멤버십 테스트 및 중복 항목 제거에 사용된다.

<br/>

##### frozen set

frozen set은 불변 타입이다. 그래서 frozen set의 요소를 변경하는 메소드를 사용할 수 없다.

```python
# frozen set의 생성
>>> s = frozenset()
>>> type(s)
"<class 'frozenset'>"
```

<br/>

##### 집합 메소드

```python
# s.add(x): 집합 s에 x가 없을 경우 x를 추가한다.
>>> number = {'one', 'two'}
>>> number.add('three')
>>> number
{'three', 'two', 'one'}

# a.update(b) 또는 a|=b : 집합 a에 집합 b의 요소를 추가한다.(합집합)
>>> number.update({'four', 'five'})
>>> number
{'two', 'one', 'three', 'four', 'five'}
>>> number |= {'six'}
>>> number
{'two', 'one', 'three', 'four', 'five', 'six'}

# a.union(b) 또는 a|b : update()와 기능은 동일하지만 복사본을 리턴한다.
>>> number.union({'seven'})
{'two', 'seven', 'one', 'three', 'four', 'five', 'six'}
>>> number
{'two', 'one', 'three', 'four', 'five', 'six'}
>>> number | {'seven'}
{'two', 'seven', 'one', 'three', 'four', 'five', 'six'}

# a.intersection(b) 또는 a&b : 집합 a와 집합 b의 교집합의 복사본을 리턴한다.
>>> number2 = {'one', 'two'}
>>> number.intersection(number2)
{'two', 'one'}
>>> number & number2
{'two', 'one'}
>>> number
{'two', 'one', 'three', 'four', 'five', 'six'}

# a.difference(b) 또는 a-b : 집합 a와 집합 b의 차집합의 복사본을 리턴한다.
>>> number.difference(number2)
{'three', 'four', 'five', 'six'}
>>> number - number2
{'three', 'four', 'five', 'six'}
>>> number
{'two', 'one', 'three', 'four', 'five', 'six'}

# a.clear(): 집합 a의 모든 요소를 제거한다.
>>> number2.clear()
>>> number2
set()

# a.discard(x): 집합 a에서 항목 x를 제거하며 반환값은 없다.
# a.remove(x): 기능은 discard()와 동일하며 항목 x가 없을 경우 KeyError를 발생시킨다.
# a.pop(): 집합 a에서 한 항목을 무작위로 제거하고 그 항목을 리턴한다. 빈 집합일 경우 KeyError가 발생.
>>> number.discard('one')
>>> number
{'two', 'three', 'four', 'five', 'six'}
>>> number.remove('two')
>>> number
{'three', 'four', 'five', 'six'}
>>> number.remove('seven')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'seven'
>>> number.pop()
'three'
>>> number
{'four', 'five', 'six'}
```

참고로, dictionary의 keys(), items() 메소드와 함께 합집합, 교집합, 차집합 연산을 사용할 수 있다. 하지만 values() 메소드와는 사용할 수 없다. 정확히는 keys(), items()의 리턴값인 Dictionary view objects에는 합집합, 교집합, 차집합 연산을 적용할 수 있으나, values()의 리턴값인 Dictionary view objects에는 이들 연산을 적용할 수 없다.

<br/>

<br/>

##### 딕셔너리(dictionary)

파이썬의 딕셔너리는 key를 통해 이와 연관된 value를 얻는 해시 테이블(hash table)로 구현되어 있다. 해시 함수가 key로 사용되는 객체에 임의의 정수값을 상수시간 내에 계산하여 부여하면, 이 정수는 해당 key와 연관되어 있는 value를 찾는데 사용된다.

```python
>>> hash(35)
35
>>> hash('python')
-4462428303321025586
```

python built-in mapping type인 딕셔너리는 반복가능(iterable)하고, 가변(mutable) 타입이며, 멤버십 연산자 in과 길이를 구하는 len() 함수를 지원한다.

딕셔너리의 key 값은 중복되지 않으며, key를 통해 value에 접근하는 시간복잡도는 O(1)이다. 왜냐하면 key가 해시함수를 통과하여 만들어진 고유의 정수를 가지고 바로 value에 접근하기 때문이다. 반면에 리스트는 처음부터 하나하나 비교를 해서 찾아가기 때문에 O(n)의 시간복잡도를 가진다.

| 연산             | 시간복잡도 |
| ---------------- | ---------- |
| 복사             | O(n)       |
| 항목 조회        | O(1)       |
| 항목 할당        | O(1)       |
| 항목 삭제        | O(1)       |
| 멤버십 테스트 in | O(1)       |
| 반복             | O(n)       |

딕셔너리는 인덱스로 값에 접근하는 타입이 아니기 때문에 슬라이스를 사용할 수 없다.

파이썬 3.6까지의 딕셔너리는 항목의 삽입 순서를 기억하지 않았으나, 파이썬 3.7부터는 항목의 삽입 순서를 기억한다.

```python
# 딕셔너리를 생성하는 여러 방법
>>> hotel = {'room':30, 'employee':1000}
>>> hotel
{'room': 30, 'employee': 1000}

>>> hotel = dict(room=40, employee=2000, number='A2')
>>> hotel
{'room': 40, 'employee': 2000, 'number': 'A2'}

>>> man = dict([('name','Park'), ('age', 30)])
>>> man
{'name': 'Park', 'age': 30}

>>> man = {}
>>> man['name'] = 'Kim'
>>> man['age'] = 40
>>> man
{'name': 'Kim', 'age': 40}
```

<br/>

##### 딕셔너리 메소드

```python
"""
a.setdefault(key, default): 딕셔너리 a에 key가 존재할 경우 key에 해당하는 value를 리턴하고,
key가 존재하지 않으면 해당 key를 새로운 key로 하고 default를 새로운 value로 하여 딕셔너리에 저장한 후,
default를 리턴한다.
"""
def usual_dict(dict_data):
    """ dict[key] 사용 """
    newdata = {}
    for k, v in dict_data:
        if k in newdata:
            newdata[k].append(v)
        else:
            newdata[k] = [v]
    return newdata

def setdefault_dict(dict_data):
    """ setdefault() 메서드 사용 """
    newdata = {}
    for k, v in dict_data:
        newdata.setdefault(k, []).append(v)
    return newdata

def test_setdef():
    dict_data = (("key1", "value1"),
                 ("key1", "value2"),
                 ("key2", "value3"),
                 ("key2", "value4"),
                 ("key2", "value5"),)
    print(usual_dict(dict_data))
    print(setdefault_dict(dict_data))

if __name__ == "__main__":
    test_setdef()
    
"""
실행결과
{'key1': ['value1', 'value2'], 'key2': ['value3', 'value4', 'value5']}
{'key1': ['value1', 'value2'], 'key2': ['value3', 'value4', 'value5']}
"""
```

```python
"""
a.update(b): 딕셔너리 a에 딕셔너리 b의 키가 존재한다면, 기존 a의 (키, 값)을 b의 (키, 값)으로 갱신한다.
b의 키가 a에 존재하지 않는다면 b의 (키, 값)을 a에 추가한다.
"""
>>> d = {'a':1, 'b':2}
>>> d.update({'b':10})
>>> d
{'a': 1, 'b': 10}
>>> d.update({'c':100})
>>> d
{'a': 1, 'b': 10, 'c': 100}

# a.get(key): 딕셔너리 a에서 key에 해당하는 value를 리턴한다. key가 없으면 아무것도 리턴하지 않는다.
>>> d.get('b')
10
>>> d.get('d')  # 딕셔너리에 키가 없으면 아무것도 리턴하지 않음
>>> d['a']  # 이런 방식으로도 value를 가져올 수 있음
1
>>> d['d']  # 이 방식의 경우 딕셔너리에 키가 없으면 KeyError를 발생시킴
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'd'
```

items(), keys(), values() 메소드는 Dictionary view object 를 리턴한다. Dictionary view object 는 딕셔너리 엔트리(key, value)의 dynamic view를 제공하는데, 이는 딕셔너리의 엔트리가 변경되면 view object는 변경된 것을 반영하여 보여준다는 것을 의미한다. view object는 읽기전용의 반복 가능(iterable)한 객체이며 멤버십 테스트를 지원한다.

```python
>>> student = dict(name='Park', age=30, grade='A+')
>>> student.items()
dict_items([('name', 'Park'), ('age', 30), ('grade', 'A+')])
>>> student.keys()
dict_keys(['name', 'age', 'grade'])
>>> student.values()
dict_values(['Park', 30, 'A+'])

>>> a = student.keys()  # 변수 a에 key들을 모아놓은 view object를 저장
>>> a
dict_keys(['name', 'age', 'grade'])
>>> student['hobby'] = 'coding'  # 딕셔너리에 엔트리를 추가
>>> a  # 변수 a에 저장되어 있는 view object를 보면 딕셔너리의 변경이 반영되어 있음을 알 수 있음
dict_keys(['name', 'age', 'grade', 'hobby'])

# view object는 읽기전용
>>> a['age'] = 20
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'dict_keys' object does not support item assignment
```

```python
"""
a.pop(key, default): 딕셔너리 a에 key가 있으면 해당 엔트리를 제거하고 key의 value를 리턴한다.
딕셔너리에 key가 없다면 default를 리턴한다.
만약 default를 명시하지 않았고, key도 딕셔너리에 없다면 KeyError를 발생시킨다.
a.popitem(): 딕셔너리 a에서 엔트리 하나를 제거하고 (key, value) 형태로 리턴한다.
제거순서는 LIFO(Last In First Out)이고, 딕셔너리가 비어있다면 KeyError를 발생시킨다.
"""
>>> student = {'name': 'Park', 'age': 30, 'grade': 'A+', 'hobby': 'coding'}
>>> student.pop('age')
30
>>> student.pop('height','no key')
'no key'
>>> student
{'name': 'Park', 'grade': 'A+', 'hobby': 'coding'}

>>> student.popitem()
('hobby', 'coding')
>>> student.popitem()
('grade', 'A+')
>>> student
{'name': 'Park'}

# a.clear(): 딕셔너리의 모든 항목을 제거한다.
>>> student.clear()
>>> student
{}
```

<br/>

##### 딕셔너리의 순회

반복문에서 딕셔너리를 순회할 때는 기본적으로 key를 사용한다. 파이썬 3.6 까지는 임의의 순서대로 나타나므로 sorted() 함수를 사용하여 정렬한 후에 순회하면 되고, 파이썬 3.7 부터는 삽입순서를 기억하기 때문에 삽입순서대로 순회 할 수 있다.

```python
>>> d = {'a':'apple', 'b':'banana', 'c':'cat'}
>>> for k in d.keys():
...     print(k, d[k])
...
a apple
b banana
c cat
```

<br/>

##### 딕셔너리의 분기

만약 두 함수를 조건에 따라 실행해야한다고 가정해보자. if, elif 문을 사용해서 작성할 수 있을 것이다. 하지만 딕셔너리를 이용해서도 작성할 수 있다.

```python
def hello():
    print('hello')
    
def world():
    print('world')
    
action = 'h'
functions = dict(h=hello, w=world)
functions[action]()
```

위와 같이 작성할 수 있는 이유는 파이썬이 **First Class Function** 을 지원하기 때문이다. First Class Function 이란 프로그래밍 언어가 함수를 first class citizen으로 취급하는 것을 말한다. 자세히 설명하자면, 함수 자체를 또 다른 함수의 인자(argument)로 전달할 수 있고, 리턴값으로 함수를 줄 수 있으며, 함수를 변수에 할당하여 데이터 구조 안에 저장할 수 있음을 말한다.

위의 코드에서 딕셔너리에는 hello와 world 함수 객체가 value로 저장되어 있는 것이다. 정확히는 함수가 저장되어있는 메모리 주소값이 저장된다. 그래서 action key를 이용하여 특정 함수 객체를 리턴받아 실행할 수 있다.

<br/>

##### 딕셔너리의 언패킹

**언패킹(unpacking)**은 여러개의 객체를 갖고 있는 하나의 객체를 여러개로 나누어 주는 것을 의미한다. 반대로 **패킹(packing)**은 여러개의 객체를 하나의 객체로 묶어주는 것을 의미한다.

keyword가 있는 collection인 딕셔너리를 언패킹 할 수 있는 연산자는 '**'이다. 다음과 같이 딕셔너리를 언패킹하여 함수의 인자로 전달할 수 있다.

```python
>>> a = {"name" : "Park", "age" : 30}
>>> "name: {name}, age: {age}".format(**a)
'name: Park, age: 30'
```

<br/>

<br/>

##### collections 모듈의 dictionary types

파이썬의 collections 모듈에는 built-in dictionary type보다 더 강력한 기능의 dictionary types가 있다.

<br/>

**default dictionary (collections.defaultdict)** 은 내장 딕셔너리의 모든 연산자와 메소드를 사용할 수 있으며, 추가로 missing value에 대해 지정한 default 값을 제공해준다.

```python
from collections import defaultdict

def defaultdict_example():
    pairs = {("a", 1), ("b", 2), ("c", 3)}

    # 일반 딕셔너리
    d1 = {}
    for key, value in pairs:
        if key not in d1:
            d1[key] = []
        d1[key].append(value)
    print(d1)

    # defaultdict
    d2 = defaultdict(list)  # default value를 빈 list로 지정
    for key, value in pairs:
        d2[key].append(value)  # 딕셔너리에 key가 없으면 (key, default value)를 생성해준다.
    print(d2)

if __name__ == "__main__":
    defaultdict_example()
    
"""
실행결과
{'b': [2], 'a': [1], 'c': [3]}
defaultdict(<class 'list'>, {'b': [2], 'a': [1], 'c': [3]})
"""
```

<br/>

**ordered dictionary (collections.OrderedDict)** 은 내장 딕셔너리의 모든 연산자와 메소드를 사용할 수 있으며, 엔트리의 삽입순서를 기억한다. 그래서 키 값을 변경해도 순서는 변경되지 않는다. 항목을 맨 끝으로 저장하려면 해당 항목을 삭제한 후 다시 삽입해야 한다.

일반적으로 ordered dictionary는 딕셔너리를 여러번 순회하는 경우나 항목의 삽입을 거의 수행하지 않는 경우에만 효율적이다.

```python
from collections import OrderedDict

def orderedDict_example():
    pairs = [("c", 1), ("b", 2), ("a", 3)]

    # 일반 딕셔너리, python 3.7 version
    d1 = {}
    for key, value in pairs:
        if key not in d1:
            d1[key] = []
        d1[key].append(value)
    for key in d1:
        print(key, d1[key])

    # OrderedDict
    d2 = OrderedDict(pairs)
    for key in d2:
        print(key, d2[key])

if __name__ == "__main__":
    orderedDict_example()
    
"""
실행결과
c [1]
b [2]
a [3]
c 1
b 2
a 3
"""
```

<br/>

**counter (collections.Counter)** 는 내장 딕셔너리의 모든 연산자와 메소드를 사용할 수 있으며, 해시가능(hashable)한 객체를 카운팅 할 수 있다. 카운터에는 딕셔너리의 key로는 elements가 저장되고, 딕셔너리의 value로는 element의 갯수가 저장된다.

```python
from collections import Counter

def counter_example():
    """ element의 발생 횟수를 매핑하는 딕셔너리를 생성한다. """
    seq1 = [1, 2, 3, 5, 1, 2, 5, 5, 2, 5, 1, 4]
    seq_counts = Counter(seq1)
    print(seq_counts)

    """ element의 발생 횟수를 수동으로 갱신하거나, update() 메소드로 갱신할 수 있다. """
    seq2 = [1, 2, 3]
    seq_counts.update(seq2)
    print(seq_counts)

    seq3 = [1, 4, 3]
    for key in seq3:
        seq_counts[key] += 1
    print(seq_counts)

    """ a+b, a-b와 같은 집합 연산을 사용할 수 있다. """
    seq_counts_2 = Counter(seq3)
    print(seq_counts_2)
    print(seq_counts + seq_counts_2)
    print(seq_counts - seq_counts_2)

if __name__ == "__main__":
    counter_example()
    
"""
실행결과
Counter({5: 4, 1: 3, 2: 3, 3: 1, 4: 1})
Counter({1: 4, 2: 4, 5: 4, 3: 2, 4: 1})
Counter({1: 5, 2: 4, 5: 4, 3: 3, 4: 2})
Counter({1: 1, 4: 1, 3: 1})
Counter({1: 6, 2: 4, 3: 4, 5: 4, 4: 3})
Counter({1: 4, 2: 4, 5: 4, 3: 2, 4: 1})
"""
```

