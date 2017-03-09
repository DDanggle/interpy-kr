# 21. Targeting Python 2+3

대부분의 경우에는 파이썬 2와 3에서 모두 돌아가는 프로그램을 만들고 싶을 것 입니다.

수백명의 사람들이 사용하는 인기가 엄청난 파이썬 모듈을 작성했고, 이용자 전부가 파이썬 2 혹은 3을 가지고 있지는 않다고 생각 해보세요. 이런 때에는 두 가지 방법이 있습니다. 하나는  파이썬 2용, 파이썬 3용으로 두 개의 모듈을 배포하는 것입니다. 다른 방법은 현재 코드를 수정해서 파이썬2와 3 모두에서 잘 돌아가는 코드를 만드는 것입니다.

이 장에서는 모두 호환되는 스크립트를 만들 수 있도록 몇 가지 비법들을 소개하겠습니다.

##### Future imports

가장 중요하고 또 가장 먼저 해야할 일은 `__future__` 임포트를 사용하는 것입니다. 이것은 파이썬2에서 파이썬3의 기능을 가져올 수 있게 합니다. 몇 가지 예를 들어보겠습니다.

Context manager은 파이썬 2.6 이상에서 새로 나온 것입니다. 파이썬 2.5+에서 쓰고 싶다면,

```python
    from __future__ import with_statement
```

파이썬3에서 `print`는 함수로 변경되었습니다. 파이썬 2에서 사용하고 싶다면 `__future__`에서 임포트 할 수 있습니다.

```python
print
# Output:

from __future__ import print_function
print(print)
# Output: <built-in function print>
```

##### 모듈 재명명해서 사용하기

보통 어떻게 패키지들을 임포트하나요? 대부분 이렇게 사용할 것입니다.

```python
import foo
# or
from foo import bar
```

이렇게도 사용이 가능하다는 것도 알고계시나요?

```python
import foo as foo
```

그 위에 나열된 코드와 동일하게 보인다는 것은 알지만, 이 방법은 파이썬 2와 3에서 호환가능하게 해주는 필수 요소입니다.  아래의 코드를 보도록 하겠습니다.

```python
 try:
     import urllib.request as urllib_request # 파이썬3에서
 except:
     import urllib2 as urllib_request # 파이썬 2에서
```

위의 코드를 조금 더 설명하면, `try/except`문으로 감싸서 코드를 임포트했습니다. 그 이유는 파이썬 2에서는 urllib.request 모듈이 없어서 ImportError가 발생할 것입니다. urllib.request의 기능은 파이썬 2에서는 urllib2 모듈로 제공됩니다. 그래서 파이썬2에서 사용할 수 있도록 `urllib.request`를 임포트해보고 `ImportError`가 발생하면, \`urllib'로 임포트하라고 파이썬에게 알려주는 것입니다.

마지막으로 여러분이 알아야하는 것은 `as` 키워드입니다. 이것은 `urllib_request`로 임포트된 모듈을 명명합니다. 그래서 `urllib2`의 모든 클래스와 메소드는 `urllib_request`로 이용할 수 있습니다.

##### 더 이상 사용되지 않는 파이썬 2 내장함수

또 하나 기억해야 할 것은 파이썬3 에서는 파이썬2의 내장 함수 12개가 제거 되었다는 것입니다. 파이썬3에서 호환가능하게 코드를 만들기 위해서는, 파이썬 2에서 이 내장함수를 사용해서는 안됩니다. 강제로 이 파이썬2 내장함수 12개를 버리는 방법이 있습니다.

```python
from future.builtins.disables import *
```

여러분들이 파이썬3에서 없어진 내장함수를 사용하려 한다면, 아래와 같이 `NameError`을 발생시킬 것입니다.

```
from future.builtins.disabled import *

apply()
# Output: NameError : obsolete Python 2 builtin apply is disabled
```

##### 외부 표준 라이브러리 백포트

파이썬 2에서 파이썬 3의 기능이 제공되는 널리 사용되는 패키지가 있습니다.예를 들면,

* enum `pip install enum34`
* singledispatch `pip install singledispatch`
* pathlib `pip install pathlib`

추가로, 파이썬 2와 3에 호환가능하도록 코드를 만들 수 있도록 파이썬 공식문서는 [이해하기 쉬운 가이드](https://docs.python.org/3/howto/pyporting.html)를 제공합니다.

