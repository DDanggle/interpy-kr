# 23. Function caching

_\(역자 주: 캐싱이라는 말이 적절한지는 모르겠지만, 이 책에서는 캐시를 동사로 많이 표현하였습니다. 하단의 캐싱은, '캐시에 저장하다' 의미로 해석 해주시면 좋을 것 같습니다.\)_

함수 캐싱은 전달 인자에 따른 함수의 반환 값들을 캐싱합니다. I/O 바운드함수가 정기적으로 같은 전달 인자를 호출하는 함수일 때 시간을 아낄 수 있습니다. 파이썬 3.2 이전 버전에서는 커스텀향상을 해야했지만, 파이썬 3.2+ 부터는 `lru_cache` 데코레이터를 사용해서 쉽게 함수의 반환값들을 캐싱 혹은 언캐싱 할 수 있습니다.

**파이썬 3.2+ 이상에서의 사용방법과, 그 전 버전의 사용방법에 대해서 알아봅시다.**

### **23.1 python3.2+**

**피보나치를 계산하는 함수를 `lru_cache`를 사용해서 향상시켜봅시다.**

```python
from functools import lru_cache

@lru_cache(maxsize=32)
def fib(n):
    if n < 2:
        return n
    return fib(n-1) + fib(n-2)

print([fib(n) for n in range(10)])
#Output: [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
````

`Maxsize`인자는 `lru_cache`에 캐싱해야할 반환값의 양을 알려줍니다.

아래와 같이 리턴 값들을 언캐싱할 수도 있습니다. 

```python
fib.cache_clear()
```

### **23.2 Python 2+**

동일한 효과를 내는 여러가지 방법들이 있습니다. 직접 캐싱 매커니즘 타입 중 하나를 만들 수도 있습니다. 전적으로 필요에 따라 만들어 쓰면 됩니다. 아래의 예시는 일반적인 캐시입니다.

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

Django에서 `lru_cache`를 이용해서 버그를 잡고 있는 Caktus그룹의 [좋은 글](https://www.caktusgroup.com/blog/2015/06/08/testing-client-side-applications-django-post-mortem/)이 있습니다. 흥미로운 읽을거리이니 한 번 확인 해보시기를.

