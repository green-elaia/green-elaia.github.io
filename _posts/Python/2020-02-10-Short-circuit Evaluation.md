---
layout: post
title: 논리연산자 and, or 단락평가 (Short-circuit Evaluation)
category: Python
use_math: true
tag: [논리연산자, and, or, 단락평가]
show_sidebar: false
---



## 단락평가 (Short-circuit Evaluation)

단락평가는 논리연산자를 이용해서 True, False를 판별할 때, 첫번째 값만으로 결과가 확실하다면 두번째 값은 확인하지 않는 방식을 말한다.

예를 들어, A and B 라는 식이 주어졌을 때 A가 False라면 두번째 값인 B는 확인하지 않는다. 논리연산자 and는 두 값이 모두 True일 때 결과가 True이므로 첫번째 값이 False라면 결과는 확실히 False이다. 그래서 두번째 값은 확인할 필요가 없어진다.

마찬가지로 A or B 의 경우, A가 True라면 두번째 값인 B는 확인하지 않는다. 논리연산자 or는 두 값 중 적어도 하나가 True이면 결과가 True이기 때문이다.

<br/>

##### 파이썬에서 논리연산자는 마지막으로 단락평가를 실시한 값을 그대로 리턴한다.

A and/or B 를 하였을 때 return 되는 값이 항상 True/False가 아닐 수도 있다는 말이다.

```python
>>> True and 'korea'
'korea'
```

위의 예를 보면 첫번째 값이 True이므로 논리연산자 and는 두번째 값까지 확인하게 된다. 파이썬에서는 빈문자열은 False, 나머지는 True로 판단하기 때문에 결과는 True로 나올 것 같았지만 그렇지 않았다. 그 이유는 파이썬에서 논리연산자는 마지막으로 단락평가를 실시한 값을 그대로 리턴하기 때문이다.

```python
# 첫번째 값만으로 단락평가가 완료될 경우 리턴값
>>> 0 and 'korea'
0
>>> 111 or 'korea'
111

# 두번째 값까지 확인해야 단락평가가 완료될 경우 리턴값
>>> '1억' and '1조'
'1조'
>>> "" or 'good!'
'good!'
>>> "" or 0
0
```

