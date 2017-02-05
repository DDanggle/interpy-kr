# 24. Context manager

컨텍스트 매니저는 원하는 타이밍에 정확하게 리소스를 할당하고 제공하는 역할을 합니다. 가장 많이 사용되는 컨텍스트 매니저는 `with` 문입니다. 코드 블록 사이에서 한 쌍으로 함께 실행되어야 하는 연결된 수행 코드 2개를 가지고 있다고 생각 해보세요. 컨텍스트 매니저는 정확히 이런 것들을 가능하게 합니다. 예를 들면,

```python
with open('some_file', 'w') as opened_file:
    opened_file.write('Hola')
```

위 코드는 파일을 열고 어떤 데이터들을 쓰고 닫는 코드입니다. 만약 데이터를 쓰는 동안 에러가 발생한다면, 파일을 닫으려고 시도할 것입니다. 위 코드는 아래의 코드와 동일한 수행을 하는 코드입니다.

```python
file = open('somefile', 'w')
try:
    file.write('Hola')
finally:
    file.close()
```

첫 번째 예시와 비교해보면 간단히 `with`을 사용함으로써 많은 표준 코드들을 삭제하였습니다. `with` 문을 사용해서 가장 큰 장점은 감싸진 블록이 존재하든지 관심을 가지지 않아도 파일은 확실히 닫아진다는 것입니다.

일반적으로 파일을 잠그거나 해제하거나, \(이미 보여드렸듯\) 열려있는 파일을 닫을 때 컨텍스트 매니저를 사용합니다.

위 컨텍스트 매니저를 향상시켜보겠습니다. 뒤에서 어떻게 동작하는지 정확히 이해하도록 해줄 것입니다.

### 24.1 Class로 Context Manager 향상시키기.

최소한 컨텍스트 매니저는 `__enter__`과 `__exit__`메소드를 가지고 있습니다. 파일을 여는 컨텍스트 매니저를 만들어보면서 기본에 대해서 살펴봅시다.

```python
class File(object):
    def __init__(self, file_name, method):
        self.file_obj = open(file_name, method)
    def __enter__(self):
        return self.file_obj
    def __exit__(self, type, value, trace_back):
        self.file_obj.close()
```

방금 정의한 `__enter__` 문과 `__exit__` 문을 with문으로 사용할 수 있습니다. 다음을 실행해보겠습니다.

```python
with File('demo.txt', 'wb') as opened_file:
    opened_file.wirte('Hola!')
```

`__exit__` 함수는 세 가지 전달 인자를 받습니다.  컨텍스트 매니저 class의 일부인 모든 `__exit__` 메소드에 필요합니다. 뒤에서 어떤 일이 발생하는 지 알아봅시다.

1. `with`문은 `File`class의 `__exit__` 메소드를 저장합니다.
2. 이것은 `File` class의 `__enter__메소드`를 호출합니다.
3. '**enter**\` 메소드는 파일을 열고 파일을 반환합니다.
4. 열려진 파일 처리는 `opened_file`을 통과합니다.
5. `.write()`를 사용해서 파일을 쓸 수 있습니다.
6. `with`문은 저장된 `__exit__` 문을 호출합니다.
7. `__exit__`문은 파일을 닫습니다.

### 24.2 handling exception

아직까지 `__exit__` 메소드의 인자인 `type`, `value`, `trace_back`에 대해서는 아직 이야기를 나누지 않았습니다. 4번째에서 6번째 단계 사이에서 예외가 발생한다면, 파이썬은 예외에서의 `type`, `value`, `trace_back`을  
`__exit__`메소드에 통과시킵니다. 그러면 `__exit__` 메소드에서는 다음에 어떤 단계들이 요구되는 지에 따라 파일을 닫을 수 있는 방법을 결정해야합니다. 이 경우에는 거기까지는 생각하지 않겠습니다.

어떤 파일 객체에서 예외를 발생한다면 어떻게 될까요? 파일 객체가 지원되지 않는 메소드에 접근하려 한다고 생각해보겠습니다. 예를 들면,

```python
with File('demo.txt', 'wb') as opened_file:
    opened_file.undefined_function('Hola!')
```

`with`문이 에러를 만나면, 어떤 식으로 처리되는 지 단계들을 살펴보겠습니다.

1. type, value, trace_back의 에러가 \`\_exit_\`메소드를 통과합니다.
2. `__exit__` 메소드가 예외를 처리하도록 합니다.
3. `__exit__` 메소드가 True 를 반환한다면, 예외는 적절하게 된 것입니다.
4. `__exit__` 메소드가 True가 아닌 다른 것을 반환하면, `with`문에서 예외가 발생합니다.

위의 경우에는 `__exit__` 메소드는 None\(리턴 문이 어떤 것도 만나지 않는다면 `None`을 반환합ㄴ디ㅏ\)을 반환합니다. 그래서, `with`문은 예외를 발생시킵니다.

```
￼Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
AttributeError: 'file' objects has no attribute 'undefined_function'
```

`__exit__` 메소드에서 예외를 처리 해보겠습니다.

```python
class File(object):
    def __init__(self, file_name, method):
        self.file_obj = open(file_name, method)
    def __enter__(self):
        return self.file_obj
    def __exit__(self, type, value, trace_back):
        print "예외는 처리되어야 합니다"
        self.file_obj.close()
        return True

with File('demo.txt', 'wb') as opened_file:
    opened_file.undefined_function()
# Output: 예외는 처리되어야 합니다
```

`__exit__` 메소드가 True를 반환한다면, `with`문에서 에외는 발생하지 않을 것입니다.

컨텍스트 매니저를 향상시키는 방법말고도 다른 방법도 있습니다. 다음 섹션에서 알아보도록 하겠습니다.

### 24.3 제너레이터를 활용해서 컨텍스트 매니저 향상시키기.

데코레이터와 제너레이터를 활용해서 컨텍스트 매니저를 향상시킬 수도 있습니다. 파이썬은 바로 이러한 목적으로 contextlib 모듈들을 가지고 있습니다. class 대신에, 컨텍스트 매니저을 제너레이터 함수를 이용해서 향상시킬 수도 있습니다. 의미없는 예시를 보면서, 기본부터 확인 해보겠습니다.

```python
from contextlib import contextmanager

@contextmanager
def open_file(name):
    f = open(name, 'wb')
    yield f
    f.close()
```

자! 이 방법으로 컨텍스트 매니저를 좀 더 직관적이고 쉽게 향상시켰습니다! 그렇지만 이 메소드는 제너레이터, yield, 데코레이터에 관한 지식들이 필요합니다.  
이 예시에서는 일어날 수 있는 예외를 처리하지는 못합니다. 이전의 메소드와 동일한 방법으로 동작합니다.

위 메소드에 관해서 조금 더 살펴보면,

1. 파이썬이 yield 키워드를 만난다면, 일반적인 함수 대신에 제너레이터를 만들었기 때문입니다.
2. 데코레이션이 있기 때문에 컨텍스트 매니저는 함수 이름\(open\_file\)을 전달 인자로서 호출합니다.
3. `contextmanager`함수는 `GeneratorContextManager`로 감싸진 제너레이터를 반환합니다.
4. `GeneratorContextManager`는 `open_file`함수를 할당합니다. 그렇기 떄문에 `open_file` 함수를 앞으로 호출하면, 사실은 `GeneratorContextManager`을 호출하는 것입니다.

우리는 이제 모든 것을 배웠으므로, 아래와 같이 새롭게 만들어진 컨텍스트 매니저를 사용할 수도 있습니다.

```python
with open_file('some_file') as f:
    f.wirte('Hola')
```



