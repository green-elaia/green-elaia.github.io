---
layout: post
title: 제너레이터 (generator)
category: Python
use_math: true
tag: [generator, yield]
show_sidebar: false
---



제너레이터는 파이썬의 시퀀스를 생성하는 객체

제너레이터를 이용하면 시퀀스 전체를 한 번에 메모리에 생성하고 정렬할 필요가 없어진다. 덕분에 아주 큰 시퀀스도 효율적으로 순회할 수 있다.

yield를 만나면 함수를 마치지 않은 상태로 해당 요소를 반환하며, 제어를 호출한 곳으로 돌려준다.



제너레이터를 이용하여 피보나치 수열 만들기

```python
def fib_generator():
    a, b = 0, 1
    while True:
        yield b
        a, b = b, a+b

if __name__ == "__main__":
    fg = fib_generator()
    for _ in range(10):
        print(next(fg), end=' ')
        
"""
실행결과
1 1 2 3 5 8 13 21 34 55
"""
```



