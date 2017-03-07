# 20. 파이썬 C 확장

CPython이  개발자에게 제공하는 흥미로운 기능은 C 코드와 Python의 인터페이스 용이성 입니다.

개발자가 Python 코드에서 C 함수를 호출하는 데 사용하는 세 가지 주요 방법은  ctypes, SWIG 및 Python / C API가 있습니다. 각 방법마다 장점과 단점이 있습니다.

먼저 왜 C와 파이썬을 연결하고 싶습니까?

몇가지 일반적인 이유들이 있습니다.

* 당신은 속도를 원하고 C가 파이썬보다 50배 빠르다는 것을 알고 있습니다.
* 특정 레거시 C라이브러리는 당신이 원하는 대로 작동하므로 파이썬으로 다시 작성하고 싶지 않습니다.
* 특정 저수준 자원 액세스 - 메모리에서 파일 인터페이스
* 그냥 하고 싶어서

### 20.1 CTypes

Python ctypes 모듈은 아마도 Python에서 C 함수를 호출하는 가장 쉬운 방법 일 것입니다. ctypes 모듈은 DLL을로드하기위한 C 호환 데이터 유형 및 함수를 제공하므로 수정할 필요없이 C 공유 라이브러리를 호출 할 수 있습니다. C 부분은 만질 필요도 없다는 것이 이 방법에 단순성을 더해줍니다.

**예제**

두 수를 더하는 간단한 C코드 입니다.`add.c`로 저장하십시오.

```python
//2개의 수를 더하는 C 샘플 코드 - int and floats

#include <stdio.h>

int add_int(int, int);
float add_float(float, float);

int add_int(int num1, int num2){
    return num1 + num2;
}

float add_float(float num1, float num2){
    return num1 + num2;
}
```

다음으로 C 파일을 .so 파일 \(Windows의 DLL\)로 컴파일합니다. 그러면 adder.so 파일이 생성됩니다.

```python
#리눅스용
$  gcc -shared -Wl,-soname,adder -o adder.so -fPIC add.c

#맥용
$ gcc -shared -Wl,-install_name,adder.so -o adder.so -fPIC add.c
```

파이썬 코드

```python
from ctypes import *

#load the shared object file
adder = CDLL('./adder.so')

#Find sum of integers
res_int = adder.add_int(4,5)
print "Sum of 4 and 5 = " + str(res_int)

#Find sum of floats
a = c_float(5.5)
b = c_float(4.1)

add_float = adder.add_float
add_float.restype = c_float
print "Sum of 5.5 and 4.1 = ", str(add_float(a, b))
```



