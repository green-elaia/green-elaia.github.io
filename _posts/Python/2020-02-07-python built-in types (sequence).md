---
layout: post
title: python built-in types (sequence)
category: Python
use_math: true
tag: [string, list, tuple, byte, byte array]
show_sidebar: false
---



## Sequence Types

파이썬 sequence types에는 문자열(string), 튜플(tuple), 리스트(list), 바이트(byte), 바이트배열(byte array)이 있다. 참고로 표준 라이브러리인 collections 모듈에는 데이터의 type name과 field name을 정해서 튜플의 각 포지션에 의미를 부여할 수 있는 네임드 튜플이 존재한다.

```python
# 각 시퀀스 데이터 타입의 생성
>>> s = ""
>>> type(s)
"<class 'str'>"
>>> t = ()
>>> type(t)
"<class 'tuple'>"
>>> l = []
>>> type(l)
"<class 'list'>"
>>> b = bytes([])
>>> type(b)
"<class 'bytes'>"
>>> ba = bytearray(b"")
>>> type(ba)
"<class 'bytearray'>"

>>> from collections import namedtuple
# 네임드 튜플 생성 namedtuple(typename, fieldname)
>>> Point = namedtuple('Point', ['x', 'y'])
>>> type(p)
"<class '__main__.Point'>"
>>> p = Point(11,22)
>>> p
Point(x=11, y=22)
>>> p[0]
11
>>> p.x + p.y
33
>>> x, y = p
>>> x, y
(11, 22)
```

<br/>

##### 시퀀스 타입의 속성

1. 맴버십(membership) 연산 가능: in 키워드 사용
2. 크기(size) 함수 사용가능: len(seq)
3. 슬라이싱(slicing) 가능: seq[n:m]
4. 반복(iteration) 가능: 반복문에 있는 데이터를 순회할 수 있음

<br/>

<br/>

##### 문자열, 튜플, 바이트는 불변 객체 타입이며 리스트, 바이트배열은 가변 객체 타입이다.

일반적으로 불변 객체 타입은 복제나 비교를 위한 조작을 단순화 할 수 있으며 성능개선에도 도움을 줄 수 있기 때문에 가변 객체 타입보다 효율적이다. 하지만 객체가 변경 가능한 데이터를 많이 가지고 있는 경우에는 가변 객체 타입이 적절하다.

<br/>

##### 가변 객체 타입의 복사는 조심해야 한다.

파이썬에서 모든 변수는 객체 참조이다. 즉, 변수는 데이터 객체가 저장되어 있는 메모리 주소값을 가지고 있는다. 만약, 변수 a가 가변 타입인 리스트 객체를 참조하고 있을 때, b = a 와 같이 변수 b에 변수 a의 값을 할당하면 변수 b도 변수 a가 참조하는 리스트 객체를 참조하게 된다. 그래서 변수 b를 통해 리스트 객체의 데이터를 수정하면 변수 a로 리스트 객체를 참조했을 때 수정된 데이터를 가진 리스트를 확인할 수 있다. 이러한 복사를 **얕은 복사(shallow copy)**라 부른다.

```python
>>> a = [1, 2, 3]
>>> b = a
>>> id(a) == id(b)
True
>>> b[0] = 4
>>> b
[4, 2, 3]
>>> a
[4, 2, 3]
```

문제는 다음과 같은 상황이다. 만약, 변수 a에 원본 데이터가 있고, 원본에 손상을 주지 않고 사용하기 위해 변수 b에 원본을 복사하는 경우가 있다고 하자. 이 경우 원본 데이터가 가변 타입이라면 b = a 이렇게 사용할 수가 없다. 변수 b를 통해 데이터를 수정하면 변수 a도 동일한 곳을 참조하고 있기 때문에 원본 데이터가 손상되기 때문이다. 이를 해결하기 위해 데이터 자체를 복사하는 방법이 있는데 이것을 **깊은 복사(deep copy)**라 부른다. 깊은 복사는 copy 모듈의 deepcopy() 함수로 수행할 수 있다.

```python
>>> from copy import deepcopy

>>> a = [1, 2, 3]
>>> b = deepcopy(a)
# '=='는 변수가 참조하는 객체의 값(value)이 같으면 True
# 그래서 서로 다른 객체를 참조해도 객체가 갖는 값이 같다면 True가 됨
>>> a == b
True
>>> a is b  # 'is'는 변수가 같은 객체(object)를 참조하고 있으면 True
False
>>> b.append(4)
>>> b
[1, 2, 3, 4]
>>> a
[1, 2, 3]
```

<br/>

<br/>

##### 슬라이싱 연산

시퀀스 객체의 일부를 가져오는 것을 뜻한다. 연산자의 구문은 다음과 같다.

- seq[시작]

- seq[시작 : 끝]

- seq[시작 : 끝 : 스텝]

```python
>>> seq = "abcde fghij"
>>> seq[1]
'b'
>>> seq[2:8]
'cde fg'
>>> seq[1:11:2]
'bd gi'
>>> seq[-1]  # index가 음수일 경우 오른쪽 끝에서부터 읽는다.
'j'
>>> seq[-2:]
'ij'
```

<br/>

<br/>

##### 문자열(string)

파이썬의 모든 객체에는 두 가지 출력 형식이 있다. 문자열(string) 형식은 사람을 위해서 설계되었고, 표현(representational) 형식은 파이썬 인터프리터에서 사용하는 문자열로 보통 디버깅할 때 사용된다. 파이썬 클래스를 작성할 때에는 문자열 표현을 정의하는 것이 중요하다.

<br/>

파이썬3부터는 모든 문자열이 일반적인 바이트가 아닌 유니코드(unicode)이다 (유니코드는 전 세계 언어의 문자를 정의하기 위한 국제 표준 코드로서 공백, 특수문자, 수학 및 기타 분야의 기호들도 포함한다). 문자열 앞에 u를 붙이면 유니코드 문자열을 생성할 수 있다.

```python
>>> u"안녕\u0020세계야 !"  # \u0020은 인덱스가 0x0020인 유니코드의 문자이다. 여기서는 공백문자임. 
'안녕 세계야 !'
```

<br/>

##### 문자열 메소드

```python
# a.join(b): 리스트 b의 모든 문자열을 문자열 a를 이용하여 하나의 문자열로 결합
>>> b = ['버거킹', '맥날', 'KFC']
>>> '_'.join(b)
'버거킹_맥날_KFC'

"""
a.ljust(width, fillchar): 문자열 a를 맨 왼쪽으로 보내고 width에서 문자열 a의 길이를 제외한 만큼을 fillchar로 채운다.
a.rjust(width, fillchar): 문자열 a를 맨 오른쪽으로 보내고 나머지는 ljust와 동일
"""
>>> nation = "korea"
>>> nation.ljust(10, '!')
'korea!!!!!'
>>> nation.rjust(10, '@')
'@@@@@korea'

# a.format(): 문자열 a에 변수를 추가하거나 형식화하는 데 사용
>>> "{0}! {1}".format("nice", "korea")
'nice! korea'
>>> "name: {who}, age: {age}".format(who="Park", age=30)
'name: Park, age: 30'
>>> "name: {who}, age: {0}".format(30, who="Park")
'name: Park, age: 30'

# format() 함수는 3개의 지정자를 갖는다.
# 지정자 s는 문자열(str) 형식, r은 표현(repr) 형식, a는 아스키코드(ascii) 형식
>>> from decimal import Decimal
>>> "{0} / {0!s} / {0!r} / {0!a}".format(Decimal("99.9"))
"99.9 / 99.9 / Decimal('99.9') / Decimal('99.9')"

# a.splitlines(): 문자열 a를, 개행문자를 기준으로 분리하여 리스트의 형태로 리턴한다.
>>> seq = "hello\npython"
>>> seq.splitlines()
['hello', 'python']

"""
a.split(t, n): 문자열 a를, 문자열 t를 기준으로 정수 n번만큼 분리하여 리스트의 형태로 리턴한다.
n이 지정되지 않으면 최대한 많이 분리하며, t를 지정하지 않으면 공백문자를 기준으로 분리한다.
a.rsplit(t, n): 문자열을 오른쪽에서부터 분리하기 시작한다.
"""
>>> seq = "seoul@suwon@daejeon"
>>> seq.split('@')
['seoul', 'suwon', 'daejeon']
>>> seq.rsplit('@', 1)
['seoul@suwon', 'daejeon']

# a.strip(b): 문자열 a 앞뒤의 문자열 b를 제거한다. 문자열 b를 지정하지 않으면 공백문자를 제거한다.
# a.lstrip(b): 문자열 a의 앞쪽에 있는 문자열 b를 제거한다. 문자열 b를 지정하지 않으면 공백문자를 제거
# a.rstrip(b): 문자열 a의 뒤쪽에 있는 문자열 b를 제거한다. 문자열 b를 지정하지 않으면 공백문자를 제거
>>> seq = "@@suwon@@"
>>> seq.strip('@')
'suwon'
>>> seq.lstrip('@')
'suwon@@'
>>> seq.rstrip('@')
'@@suwon'

# strip()으로 문자열 앞뒤의 공백문자, 특수문자, 숫자 등을 한번에 제거할 수 있다.
>>> import string
>>> strip = string.whitespace + string.punctuation + string.digits + "\"" + "\'"
>>> seq = "@3\" %suwon!!\' 5"
>>> seq.strip(strip)
'suwon'

# a.swapcase(): 문자열 a의 대소문자를 반전하여 복사본을 리턴
# a.capitalize(): 문자열 a의 첫 글자를 대문자로 바꾸고 복사본을 리턴
# a.lower(): 문자열 a 전체를 소문자로 바꾸고 복사본을 리턴
# a.upper(): 문자열 a 전체를 대문자로 바꾸고 복사본을 리턴
>>> seq = "Seoul and Suwon"
>>> seq.swapcase()
'sEOUL AND sUWON'
>>> seq.capitalize()
'Seoul and suwon'
>>> seq.lower()
'seoul and suwon'
>>> seq.upper()
'SEOUL AND SUWON'

"""
a.index(sub, start, end): 문자열 a의 start에서 end 범위 내에서 문자열 sub를 찾아 그 index를 리턴
a.find(sub, start, end): index()와 기능 동일
start, end 인자를 생략할 경우 문자열 전체에 대해서 탐색을 수행
index()는 탐색을 실패하면 ValueError를 발생시키고, find()는 -1을 리턴
rindex(), rfind(): 문자열의 맨 오른쪽에서부터 탐색을 시작
"""
>>> seq = "Seoul and Suwon"
>>> seq.index('and')
6
>>> seq.find('and')
6

# a.count(sub, start, end): 문자열 a의 start에서 end 범위 내에서 문자열 sub이 나온 횟수를 리턴
>>> seq = "suwon suwon suwon"
>>> seq.count('suwon')
3

"""
a.replace(old, new, maxreplace): 문자열 a에서 문자열 old를 문자열 new로 maxreplace 만큼 변경하고 복사본을 리턴
maxreplace를 지정하지 않으면 모든 old를 new로 변경
"""
>>> seq = "suwon suwon suwon"
>>> seq.replace('suwon', 'wow', 2)
'wow wow suwon'
```

<br/>

##### f-strings

기존의 %나 .format 방식에 비해 간결하고 직관적이며 속도도 빠른 문자열 표현 방식이다. 문자열 앞에 접두사 f를 붙여 사용하며, 파이썬 3.6부터 이용 가능하다.

```python
>>> name = "Park"
>>> f"He's name is {name!r}."
"He's name is 'Park'."
>>> f"He's name is {repr(name)}."
"He's name is 'Park'."
>>> from decimal import Decimal
>>> width = 10
>>> precision = 4
>>> value = Decimal("12.34567")
>>> f"result: {value:{width}.{precision}}"
'result:      12.35'
>>> from datetime import datetime
>>> today = datetime(year=2020, month=1, day=15)
>>> f"{today:%B %d, %Y}"
'January 15, 2020'
>>> number = 1024
>>> f"{number:#0x}"
'0x400'
```

<br/>

<br/>

##### 튜플(tuple)

튜플은 쉼표(,)로 구분된 값으로 이뤄진 불변 시퀀스 타입이다. 튜플의 각 위치는 객체 참조를 갖기 때문에 튜플이나 리스트 같은 객체를 중첩하여 생성할 수 있다.

```python
# 튜플 생성의 여러방식
>>> t1 = 123, 'hello'
>>> type(t1)
"<class 'tuple'>"
>>> t1 = (123, 'hello')
>>> type(t1)
"<class 'tuple'>"
>>> empty = ()  # 빈 튜플 생성
>>> type(empty)
"<class 'tuple'>"
>>> t2 = ('python',)  # 요소가 하나인 튜플 생성. 주의할 점은 꼭 쉼표(,)를 넣어줘야 한다는 것이다.
>>> type(t2)
"<class 'tuple'>"
>>> t3 = (t1, t2)  # 중첩된 튜플의 생성
>>> t3
((123, 'hello'), ('python',))
>>> len(t3)  # 튜플의 길이 == 튜플의 요소 갯수
2
>>> t3[0]  # index를 이용하여 튜플의 요소에 접근
(123, 'hello')
```

<br/>

##### 튜플 메소드

```python
# t.count(x): 튜플 t에 항목 x가 몇 개 있는지 리턴
>>> t = 1,2,3,3,3
>>> t.count(3)
3

# t.index(x): 튜플 t에서 항목 x의 index 위치를 리턴
>>> t = 1,3,5
>>> t.index(5)
2
```

<br/>

##### 튜플 언패킹(tuple unpacking)

파이썬에서 모든 반복 가능한(iterable) 객체는 시퀀스 언패킹 연산자(sequence unpacking operator) *를 사용하여 언패킹을 할 수 있다.

```python
"""
변수 x에는 맨 앞의 값이, 변수 z에는 맨 뒤의 값이 할당되고 남은 값들은 언패킹 연산자가 붙은 변수 y에 할당된다.
"""
>>> x, *y, z = (1, 2, 3, 4)
>>> x
1
>>> y
[2, 3]
>>> z
4
```

<br/>

##### 네임드 튜플(named tuple)

네임드 튜플은 collections 모듈에 들어있는 **시퀀스 데이터 타입**이다. 네임드 튜플은 튜플의 항목을 인덱스 위치뿐만 아니라 field name으로도 참조할 수 있다.

collections.namedtuple(*typename*, *field_names*, ***, *rename=False*, *defaults=None*, *module=None*)

*typename*: 사용자가 정의하는 튜플 데이터 타입의 이름. 일반적으로 왼쪽에 할당하는 변수의 이름과 동일하게 함

*field_names*: 각 항목의 이름을 지정하는 '공백으로 구분된 문자열 또는 리스트 또는 튜플'이다.

```python
>>> from collections import namedtuple

# namedtuple로 Person이라는 시퀀스 데이터 타입을 생성
>>> Person = namedtuple('Person', "name age gender")
>>> Person = namedtuple('Person', ['name', 'age', 'gender'])
>>> Person = namedtuple('Person', ('name', 'age', 'gender'))
>>> type(Person)
"<class 'type'>"

# Person type의 데이터 p를 생성
>>> p = Person(name='Park', age=30, gender='male')
>>> p
Person(name='Park', age=30, gender='male')
>>> type(p)
"<class '__main__.Person'>"

# 튜플 항목에 접근
>>> p[0]  # 인덱스로 접근
'Park'
>>> p.age  # field_name으로 접근
30

# 튜플과 마찬가지로 네임드 튜플도 불변형 객체 타입이다.
>>> p.gender = 'female'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: can't set attribute
```

<br/>

<br/>

##### 리스트(list)

여러 요소(element)들이 연속된 메모리에 순차적으로 저장되는 자료구조를 배열(array)이라 한다. 이러한 배열과 비슷한 것이 리스트이다. 즉, **파이썬의 리스트는 크기를 동적으로 조정할 수 있는 배열**이다.

가변 타입인 리스트는 대괄호 [] 안에 항목을 쉼표(,)로 구분하여 표현한다. 리스트의 항목은 각기 다른 데이터 타입이어도 된다.

```python
>>> l = ['abc', 10, [1,2]]
>>> l
['abc', 10, [1, 2]]
>>> type(l)
"<class 'list'>"

# 빈 리스트 생성
>>> l = []
>>> l = list()
```

리스트는 항목을 검색해야 하는 remove(), index(), 멤버십 테스트 in 등의 시간복잡도(time complexity)와 지정한 인덱스에 항목을 삽입한 후, 그 이후의 인덱스 항목들을 한 칸씩 뒤로 밀어야 하는 insert()의 시간복잡도가 O(n)이므로 검색이나 멤버십 테스트에서 빠른 속도를 원할 시 set이나 dictionary를 쓰는 것이 좋다. 또는 리스트를 정렬하여 보관하면 검색시간을 줄일 수 있다.

<br/>

##### 리스트 메소드

```python
# a.append(x): 리스트 a 끝에 항목 x를 추가
>>> a = [1,2]
>>> a.append(3)
>>> a
[1, 2, 3]

# a.extend(c): 리스트 a 끝에 반복 가능한 객체 c (문자열, 리스트, 튜플)를 이어붙임. a += c와 동일
>>> a.extend("abc")
>>> a
[1, 2, 3, 'a', 'b', 'c']
>>> a.extend([4,5])
>>> a.extend((6,7))
>>> a
[1, 2, 3, 'a', 'b', 'c', 4, 5, 6, 7]

# a.insert(i, x): 리스트 a의 인덱스 i 위치에 항목 x를 삽입
>>> a.insert(1,'d')
>>> a
[1, 'd', 2, 3, 'a', 'b', 'c', 4, 5, 6, 7]

# a.remove(x): 리스트 a에서 항목 x를 제거. 항목 x가 존재하지 않으면 ValueError 발생시킴
>>> a.remove(7)
>>> a
[1, 'd', 2, 3, 'a', 'b', 'c', 4, 5, 6]

# a.pop(x): 리스트 a에서 인덱스가 x인 항목을 제거하고 그 항목을 리턴
# x를 지정하지 않으면 리스트의 맨 끝 항목을 제거하고 그 항목을 리턴
>>> a.pop(1)
'd'
>>> a.pop()
6
>>> a
[1, 2, 3, 'a', 'b', 'c', 4, 5]

"""
del: 리스트 인덱스를 지정하여 특정 항목 또는 특정 범위의 항목들을 삭제. 변수 자체를 삭제할 수도 있음.
변수를 삭제했을 경우 메모리에서 리스트 객체를 삭제한 것이 아니라 리스트 객체를 참조하는 것을 삭제한 것이다.
만약 리스트 객체를 참조하는 변수가 더 이상 없다면,
리스트 객체에 할당된 메모리는 가비지 컬렉터(garbage collector)가 회수한다.
"""
>>> del a[0]
>>> a
[2, 3, 'a', 'b', 'c', 4, 5]
>>> del a[1:3]
>>> a
[2, 'b', 'c', 4, 5]
>>> del a
>>> a
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'a' is not defined

# a.index(x): 리스트 a에서 항목 x의 인덱스를 리턴
>>> a = ['a', 'b', 'c']
>>> a.index('b')
1

# a.count(x): 리스트 a에 항목 x가 몇 개 있는지를 리턴
>>> a = ['a', 'b', 'c', 'b']
>>> a.count('b')
2

# a.reverse(): 리스트 a의 항목순서를 반대로하여 그 변수 자체에 적용한다(in place).
>>> a = [1,2,3]
>>> a.reverse()
>>> a
[3,2,1]
```

**a.sort(key=None, reverse=False)**: 리스트 a의 항목을 정렬하여 그 변수 자체에 적용한다(in place).
디폴트는 오름차순 정렬이고, 인자 reverse=True 로 지정하면 내림차순 정렬이 가능하다.
리스트의 항목이 여러 데이터로 적혀있는 것이라면, 예를 들어 "연도-월-일-시간"이 문자열로 들어있을 때, 또는 (이름, 성적, 학번)이 튜플로 들어있을 때, 특정 데이터를 기준으로 정렬을 하고 싶을 때가 있을 것이다. 이 경우, 인자 key에 항목을 1차적으로 처리해줄 수 있는 함수를 지정해준다.
lambda식 함수를 지정할 수도 있고, 미리 정의한 함수의 함수명을 지정하여(함수객체의 메모리주소를 넘겨주어) 함수를 참조하도록 할 수도 있다.
참고로, **내장함수 중에 sorted()**가 있는데, 이 함수는 리스트뿐만 아니라 iterable object 모두를 정렬할 수 있다. 또한 sorted()는 in place가 아니라 정렬된 iterable 객체를 새로운 객체로 생성하여 리턴한다.

```python
>>> a = ['김씨', '최씨', '정씨', '박씨']
>>> a.sort()
>>> a
['김씨', '박씨', '정씨', '최씨']
>>> a.sort(reverse=True)
>>> a
['최씨', '정씨', '박씨', '김씨']

# time 모듈의 strptime(string, format) 함수를 이용해서 날짜를 정렬할 수 있다.
# strptime(string, format): 날짜를 표현하는 문자열을 format에 맞게 파싱(parsing) 하는 함수
>>> from time import strptime
>>> timestamp = [
... "2018-12-12 01:17:31",
... "2018-12-12 02:17:28",
... "2018-11-25 07:30:35",
... "2018-11-25 11:32:33"
... ]
>>> timestamp.sort(key=lambda x: strptime(x, '%Y-%m-%d %H:%M:%S')[0:6], reverse=True)
>>> timestamp
['2018-12-12 02:17:28', '2018-12-12 01:17:31', '2018-11-25 11:32:33', '2018-11-25 07:30:35']
```

<br/>

##### 리스트 언패킹(list unpacking)

튜플 언패킹과 방식은 동일하다.

```python
>>> a, *b, c = [1,2,3,4,5]
>>> a
1
>>> b
[2, 3, 4]
>>> c
5
```

리스트 언패킹 연산자 '*'를 이용해서 함수의 인수로 전달할 수 있다.

```python
>>> def func(a,b,c):
...     return a + b + c
...
>>> l = [1, 2, 3]
>>> func(*l)
6
>>> func(5, *l[1:])
10
>>> func(5, *l)  # 언패킹으로 인수를 전달할 때 인수의 갯수를 맞춰줘야 한다.
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: func() takes 3 positional arguments but 4 were given
```

<br/>

##### 리스트 컴프리헨션(list comprehension)

리스트 컴프리헨션은 반복문과 조건문을 이용하여 간단하게 리스트를 표현하는 방식이다. 형식은 다음과 같다.

- [ 항목 for 항목 in 반복 가능한 객체 ]
- [ 표현식 for  항목 in 반복 가능한 객체 ]
- [ 표현식 for 항목 in 반복 가능한 객체 if 조건문 ]

반복 가능한 객체(iterable object): range, list, tuple, dictionary ...

```python
>>> a = [x for x in range(10)]
>>> a
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

>>> b = [str(round(355/113.0, i)) for i in range(1,6)]
>>> b
['3.1', '3.14', '3.142', '3.1416', '3.14159']

>>> c = [y for y in range(1900, 1920) if y%4 == 0]
>>> c
[1900, 1904, 1908, 1912, 1916]

>>> words = "Python is a very interesting language".split()
>>> d = [[w.upper(), w.lower(), len(w)] for w in words]
>>> d
[['PYTHON', 'python', 6], ['IS', 'is', 2], ['A', 'a', 1], ['VERY', 'very', 4],
 ['INTERESTING', 'interesting', 11], ['LANGUAGE', 'language', 8]]
```

코드 내용의 가독성을 위해서 리스트 컴프리헨션은 단순한 경우에만 사용하는 것이 좋다. 여러개의 반복문과 조건문을 사용하여 리스트 컴프리헨션을 작성하면 코드를 이해하기 어렵게 된다. 그러므로 단순한 경우에만 리스트 컴프리헨션을 이용하고, 복잡한 경우는 여러 줄의 표현식, 반복문, 조건문으로 작성하는 것이 가독성 측면에서 볼 때 좋다.

다음은 google python style guide(https://google.github.io/styleguide/pyguide)에서 소개하는 좋은 예와 나쁜 예이다.

```python
Yes:
  result = [mapping_expr for value in iterable if filter_expr]

  result = [{'key': value} for value in iterable
            if a_long_filter_expression(value)]

  result = [complicated_transform(x)
            for x in iterable if predicate(x)]

  descriptive_name = [
      transform({'key': key, 'value': value}, color='black')
      for key, value in generate_iterable(some_input)
      if complicated_condition_is_met(key, value)
  ]

  result = []
  for x in range(10):
      for y in range(5):
          if x * y > 10:
              result.append((x, y))

  return {x: complicated_transform(x)
          for x in long_generator_function(parameter)
          if x is not None}

  squares_generator = (x**2 for x in range(10))

  unique_names = {user.name for user in users if user is not None}

  eat(jelly_bean for jelly_bean in jelly_beans
      if jelly_bean.color == 'black')
```

```python
No:
  result = [complicated_transform(
                x, some_argument=x+1)
            for x in iterable if predicate(x)]

  result = [(x, y) for x in range(10) for y in range(5) if x * y > 10]

  return ((x, y, z)
          for x in range(5)
          for y in range(5)
          if x != y
          for z in range(5)
          if y != z)
```

<br/>

##### 리스트 메소드 성능 측정

리스트를 만드는 4가지 방식(concat, append, comprehension, list & range)의 성능을 측정하고자 한다.

테스트를 위해 timeit 모듈의 Timer 객체를 생성하여 Timer 객체의 메소드인 timeit(number=1000000)를 사용할 것이다.

```python
def test1():
    l = []
    for i in range(1000):
        l = l + [i]

def test2():
    l = []
    for i in range(1000):
        l.append(i)

def test3():
    l = [i for i in range(1000)]

def test4():
    l = list(range(1000))

if __name__ == "__main__":
    import timeit
    # timeit.Timer(테스트 코드, 테스트를 위한 설정문)
    # timeit(number=1000): 테스트 코드의 1000번 실행시간을 측정하여 밀리초를 부동소수점 값으로 리턴
    t1 = timeit.Timer("test1()", "from __main__ import test1")
    print("concat ", t1.timeit(number=1000), "milliseconds")
    t2 = timeit.Timer("test2()", "from __main__ import test2")
    print("append ", t2.timeit(number=1000), "milliseconds")
    t3 = timeit.Timer("test3()", "from __main__ import test3")
    print("comprehension ", t3.timeit(number=1000), "milliseconds")
    t4 = timeit.Timer("test4()", "from __main__ import test4")
    print("list range ", t4.timeit(number=1000), "milliseconds")
    
"""
실행결과
concat  1.3792111999999999 milliseconds
append  0.07055560000000005 milliseconds
comprehension  0.04091880000000003 milliseconds
list range  0.01593089999999986 milliseconds
"""
```

실행결과, 리스트를 생성하는데 list & range, comprehension, append, concat 순으로 빠른 것을 알 수 있다.

<br/>

<br/>

##### 바이트(byte)와 바이트 배열(byte array)

파이썬은 원시 바이트(raw byte)를 처리하는 데 사용할 수 있는 데이터 타입으로 불변 타입의 바이트와 가변 타입의 바이트 배열을 제공한다. 두 타입 모두 0~255 범위의 8bit unsigned integer sequence로 이뤄진다. 바이트 타입은 문자열 타입과 비슷하고, 바이트 배열은 리스트 타입과 비슷하다.

```python
>>> l = [1, 2, 25, 255]
>>> the_bytes = bytes(l)
>>> the_bytes
b'\x01\x02\x19\xff'

>>> the_byte_array = bytearray(l)
>>> the_byte_array
bytearray(b'\x01\x02\x19\xff')

>>> the_bytes[1] = 127  # 바이트는 불변타입
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'bytes' object does not support item assignment
    
>>> the_byte_array[1] = 127  # 바이트 배열은 가변타입
>>> the_byte_array
bytearray(b'\x01\x7f\x19\xff')
```


