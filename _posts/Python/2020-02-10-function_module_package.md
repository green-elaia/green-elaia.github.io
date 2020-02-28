---
layout: post
title: 함수, 모듈, 패키지 (function, module, package)
category: Python
use_math: true
tag: [function, module, package]
show_sidebar: false
---



## Function

함수는 특정 기능을 수행하도록 작성한 코드 뭉치이다.

함수를 사용하면 코드를 간단히 재사용할 수 있으며, 프로그램의 흐름을 일목요연하게 파악할 수 있다.

파이썬에서 함수는 def를 사용하여 정의한다. def가 실행되면 함수의 객체와 참조가 같이 생성된다. 

함수의 리턴값을 정의하지 않으면 파이썬은 자동으로 None를 리턴하며, 리턴값을 정의하지 않은 함수를 프로시저(procedure)라고 한다.

<br/>

##### 스택과 활성화 레코드 (Stack and Activation Record)

함수가 호출될 때마다 활성화 레코드가 생성된다. 활성화 레코드는 함수의 정보(리턴값, 매개변수, 지역변수, 리턴주소 등)를 기록한 것을 말하며, 이는 스택에 저장된다. 여기서 스택은 프로세스에 할당된 메모리 공간에 있는 스택영역을 말한다. 스택은 함수가 호출되면 생성되고 함수가 완료되면 제거된다.

<br/>

##### 함수 매개변수의 기본값으로 가변 객체를 사용하지 않는다.

매개변수의 기본값으로 가변 객체를 사용하게 되면, 함수가 호출될 때마다 매번 새로운 기본값으로 세팅되지 않고 이전에 생성한 기본값을 참조하게 된다.

```python
# 가변 객체를 매개변수의 기본값으로 사용한 경우
>>> def append(number, number_list=[]):  # 가변 객체인 리스트를 기본값으로 사용
...     number_list.append(number)
...     return number_list
...
>>> append(3)
[3]
>>> append(5)
[3, 5]  # 예상 결과 [5]
>>> append(7)
[3, 5, 7]  # 예상 결과 [7]
```

```python
# 매개변수의 기본값으로 가변 객체를 사용하지 않은 경우
>>> def append(number, number_list=None):
...     if number_list is None:
...         number_list = []
...     number_list.append(number)
...     return number_list
...
>>> append(3)
[3]
>>> append(5)
[5]
>>> append(7)
[7]
```

<br/>

##### 내장함수 dir(*object*)

내장함수 dir()은

인자가 없을 경우, 현재 local scope의 이름들을 리스트 형태로 리턴한다.

인자가 있을 경우, 객체의 모든 속성(attribute)과 메소드를 리스트 형태로 리턴한다. 만약 객체가 __ dir __ () 메소드를 갖고 있다면 커스텀화한 __ dir __ () 메소드의 리턴값을 리턴한다.

dir() 함수를 이용하면 모듈이 정의하는 모든 유형의 이름(모듈, 변수, 함수)을 확인할 수 있다.

```python
>>> import struct
>>> dir()   # show the names in the module namespace  
['__builtins__', '__name__', 'struct']
>>> dir(struct)   # show the names in the struct module 
['Struct', '__all__', '__builtins__', '__cached__', '__doc__', '__file__',
 '__initializing__', '__loader__', '__name__', '__package__',
 '_clearcache', 'calcsize', 'error', 'pack', 'pack_into',
 'unpack', 'unpack_from']
>>> class Shape:
...    def __dir__(self):
...        return ['area', 'perimeter', 'location']
>>> s = Shape()
>>> dir(s)   # 커스텀화한 __dir__()의 리턴값을 리턴함.
['area', 'location', 'perimeter']
```

<br/>

<br/>

## Module & Package

모듈은 변수, 함수, 클래스를 모아놓은 파이썬 파일이다.

패키지는 __ init __ .py 파일과 여러 모듈을 모아놓은 디렉터리이다.

파이썬은 __ init __ .py 파일이 존재하는 디렉터리를 패키지로 인식하기 때문에 패키지로 사용하기 위해서는 __ init __ .py 파일이 필요하다. 만약 __ init __ .py 파일이 존재하지 않는다면 해당 디렉터리는 모듈 검색시 검색 경로에서 제외되어 검색되지 않는다.

```python
import 패키지디렉터리명.모듈파일명
from 디렉터리명 import 모듈파일명
```

__ init __ .py 파일은 내용이 없는 빈 파일로 만들 수도 있지만, 패키지를 import 할 때 초기화시킬 코드를 작성할 수도 있다.

 __ init __ .py 파일에는 __ all __ 변수를 정의할 수 있다. __ all __ 변수에는 패키지가 import 될 때, 패키지 내에서 default로 import 할 모듈들의 이름이 저장된다. 그래서 __ all __ 변수에 저장되지 않은 모듈은 패키지 import 시 자동으로 import 되지 않는다. 따로 명시하여 import 해야한다.

```python
__all__ = ["모듈파일명1", "모듈파일명2", ...]
```

만약, **from 디렉터리명 import *** 형태로 import 했다면,

__ all __ 변수가 없는 경우 __ 로 시작하는 모듈을 제외한 모듈의 모든 객체를 import 하고,

__ all __ 변수가 있는 경우 __ all __ 변수에 저장된 모듈의 모든 객체를 import 한다.

<br/>

##### 터미널에서 python의 -c 옵션을 이용해서 특정 모듈이 있는지 간단히 확인할 수 있다.

-c 옵션은 프로그램을 문자열로 전달한다는 의미의 옵션이다 (program passed in as string (terminates option list)).

```
$ python -c "import astin"
Traceback (most recent call last):
  File "<string>", line 1, in <module>
ModuleNotFoundError: No module named 'astin'
```

<br/>

##### __ name __ 변수

파이썬은 모듈을 import할 때마다 __ name __ 변수를 만들어 해당 모듈의 이름을 저장한다.

만약 파일을 직접 실행한다면 __ name __ 변수에는 __ main __ 이 저장된다.

```python
# helloWorld.py
if __name__ == '__main__':
    print("{0} 직접 실행됨".format(__name__))
else:
    print("{0} 임포트됨".format(__name__))
```

```
$ python
>>> import helloWorld
helloWorld 임포트됨
>>> __name__
'__main__'

$ python helloWorld.py
__main__ 직접 실행됨
```

<br/>

##### 바이트 컴파일 코드 (byte-compiled code)

일반적으로 쓰이는 파이썬은 C언어로 만들어진 CPython이다. 파이썬 프로그램의 실행은, 원시코드를 바이트 코드로 컴파일 하고, 바이트 코드를 인터프리터가 실행함으로 이루어진다.

실행코드 내에서 import 한 모듈은 컴파일러에 의해 자동으로 컴파일이 되는데, 컴파일의 결과물이 바로 바이트 컴파일 코드(.pyc)이다. 바이트 컴파일 코드는 import 한 모듈의 코드가 있는 디렉터리에 생성된다.

import 했던 모듈이 다음번에 다시 import 될 경우, 바이트 컴파일 코드가 있으면 컴파일 과정을 거치지 않기 때문에 좀더 빠르게 import 될 수 있다. 그래서 표준 모듈을 많이 사용하는 프로그램의 로딩 시간을 줄이기 위해 바이트 컴파일 코드가 사용된다.

파이썬 인터프리터를 호출할 때 -O 옵션을 주면, 최적화된 코드가 생성되어 .pyo 파일(.pyo는 최적화된 .pyc이다)로 저장된다. 최적화 과정에서 assert문과 __ debug __ 가 제거된다. 이렇게 만든 파일은 reverse engineering을 까다롭게 만들기 때문에 라이브러리로 배포하는 데 사용할 수 있다.

참고. 파이썬 확장자와 그 의미

- `.py`: This is normally the input source code that you've written.
- `.pyc`: This is the compiled bytecode. If you import a module, python will build a `*.pyc` file that contains the bytecode to make importing it again later easier and faster.
- `.pyo`: This is a `*.pyc` file that was created while optimizations (`-O`) was on.
- `.pyd`: This is basically a windows dll file.

<br/>

##### sys 모듈

**sys.path**는 인터프리터가 모듈을 검색할 경로를 담고 있는 문자열 리스트다. sys.path 변수는 PYTHONPATH 환경변수나 내장된 기본값 경로로 초기화된다. sys.path 변수에 임시로 모듈 경로를 추가하여 사용할 수도 있다.

```python
>>> import sys
>>> sys.path
['', 'C:\\ProgramData\\Anaconda3\\python37.zip', 'C:\\ProgramData\\Anaconda3\\DLLs', 'C:\\ProgramData\\Anaconda3\\lib', 'C:\\ProgramData\\Anaconda3', 'C:\\ProgramData\\Anaconda3\\lib\\site-packages', 'C:\\ProgramData\\Anaconda3\\lib\\site-packages\\win32', 'C:\\ProgramData\\Anaconda3\\lib\\site-packages\\win32\\lib', 'C:\\ProgramData\\Anaconda3\\lib\\site-packages\\Pythonwin']
>>> sys.path.append('임시 모듈 디렉터리 경로')
```

sys.path에 들어있는 경로를 보면 크게 '현재 작업 디렉터리', '표준 라이브러리가 있는 디렉터리', '서드파티(3rd party) 라이브러리가 있는 디렉터리'가 있음을 확인할 수 있다. 즉, 파이썬 인터프리터는 모듈을 검색할 때 **'현재 작업 디렉터리', '표준 라이브러리가 있는 디렉터리', '서드파티(3rd party) 라이브러리가 있는 디렉터리'**를 확인한다.

파이썬 인터프리터는 3개의 주요 모듈검색 경로 중에 '현재 작업 디렉터리'를 항상 가장 먼저 확인한다. '표준 라이브러리가 있는 디렉터리'와 '서드파티(3rd party) 라이브러리가 있는 디렉터리'는 정해져있는 순서가 없어서 매번 달라질 수 있다.

 **sys.argv 변수**는 명령줄에 전달된 인수를 프로그램 내에서 사용할 수 있도록 한다.

```python
# sys_ex.py
import sys

def main():
    for arg in sys.argv[1:]:
        print(arg)

if __name__ == "__main__":
    main()
```

```
$ python sys_ex.py arg1 arg2
arg1
arg2
```


