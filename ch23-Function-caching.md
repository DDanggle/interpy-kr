# 23. Function caching

_\(역자 주: 캐싱이라는 말이 적절한지는 모르겠지만, 이 책에서는 캐시를 동사로 많이 표현하였습니다. 하단의 캐싱은, '캐시에 저장하다' 의미로 해석 해주시면 좋을 것 같습니다.\)_

함수 캐싱은 전달 인자에 따른 함수의 반환 값들을 캐싱합니다. I/O 바인딩 함수가 동일한 인수를 사용하여 주기적으로 호출 될 때 시간을 절약 할 수 있습니다. 파이썬 3.2 이전 버전에서는 직접 구현해야 했습니다. 파이썬 3.2+ 부터는 `lru_cache` 데코레이터를 사용해서 쉽게 함수의 반환값들을 캐싱 혹은 언캐싱 할 수 있습니다.

파이썬 3.2+와  그 이전 버전에서 어떻게 사용할 수 있는지 살펴보겠습니다.

### **23.1 Python3.2+**

피보나치 계산기를 lru\_cache 를 사용해서 구현해 봅시다.

```python
from functools import lru_cache

@lru_cache(maxsize=32)
def fib(n):
    if n < 2:
        return n
    return fib(n-1) + fib(n-2)

print([fib(n) for n in range(10)])
#Output: [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

maxsize 인자는 lru\_cache에 캐싱해야할 최근 반환값의 수를 알려줍니다.

아래와 같이 리턴 값들을 쉽게 언캐싱 할 수 있습니다.

fib.cache\_clear\(\)

### **23.2 Python 2+**

동일한 효과를 내는 여러가지 방법들이 있습니다. 직접 모든 캐싱 매커니즘을 만들 수도 있습니다. 전적으로 필요에 따라 만들어 쓰면 됩니다. 아래의 예시는 일반적인 캐시입니다.

```python
from functools import wraps

def memoize(function):
    memo = {}
    @wraps(function)
    def wrapper(*args):
        if args in memo:
            return memo[args]
        else:
            rv = fuction(*args)
            memo[args] = rv
            return rv
    return wrapper

@memoize
def fibonacci(n):
    if n< 2: return n
    return fibonacci(n-1) + fibonacci(n-2)

fibonacci(25)
```

`lru_cache`로 인해 발생한 Django 버그를 잡고 있는 Caktus그룹의 [좋은 글](https://www.caktusgroup.com/blog/2015/06/08/testing-client-side-applications-django-post-mortem/)이 있습니다. 흥미로운 읽을거리이니 한 번 확인 해 보시기를.

