# 10. \_\_**slots\_\_ 마법**

파이썬에서 모든 클래스들은 인스턴스 속성을 가질 수 있습니다. 기본적으로 파이썬에서는 객체의 인스턴스 속성을 저장하기위해 dict형을 사용합니다.  
이는 런타임에 임의의 새 속성을 설정할 수 있으므로 정말 유용합니다

그러나 알려진 속성을 가진 소규모 클래스의 경우 병목 현상이 발생할 수 있습니다. dict형은 메모리를 많이 사용합니다. 파이썬은 객체 생성시에 모든 속성들을 저장하기 위해 메모리에 고정된 양을 할당할 수 없습니다. 그러므로 많은 양의 객체를 생성 하면 많은 메모리를 낭비하게됩니다. 이 문제를 우회하는 방법이 있습니다. \_\_slots\_\_을 사용해서 파이썬이 dict형을 사용하지 않도록 하고 고정된 속성의 집합에만 메모리를 할당하도록 합니다. 여기 `__slots__` 을 사용한 예제와 사용하지 않은 예제를 살펴보겠습니다.

Without `__slots__`:

```python
class MyClass(object):
    def __init__(name, class):
        self.name = name
        self.class = class
        self.set_up()
    # ...
```

With `__slots__`:

```python
class MyClass(object):
    __slots__ = ['name', 'class']

    def __init__(name, class):
        self.name = name
        self.class = class
        self.set_up()
    # ...
```

두 번째 코드는 메모리 사용을 줄입니다. 어떤 사람은 이 방법을 사용해서 메모리 사용량을 거의 40에서 50% 정도로 줄였습니다.

다른 방법으로는 PyPy를 사용해 볼 수도 있습니다. 이러한 모든 최적화 작업을 기본적으로 수행합니다.

아래에 [https://github.com/ianozsvald/ipython\_memory\_usage](https://github.com/ianozsvald/ipython_memory_usage) 에서 제공해주는 코드를 통해서 IPython에서 \_\_slots\_\_ 사용할 때와 사용하지 않을 때의 정확한 메모리 사용량을 보여주는 예제를 볼 수 있습니다.

```python
Python 3.4.3 (default, Jun  6 2015, 13:32:34)
Type "copyright", "credits" or "license" for more information.

IPython 4.0.0 -- An enhanced Interactive Python.
?         -> Introduction and overview of IPython's features.
%quickref -> Quick reference.
help      -> Python's own help system.
object?   -> Details about 'object', use 'object??' for extra details.

In [1]: import ipython_memory_usage.ipython_memory_usage as imu

In [2]: imu.start_watching_memory()
In [2] used 0.0000 MiB RAM in 5.31s, peaked 0.00 MiB above current, total RAM usage 15.57 MiB

In [3]: %cat slots.py
class MyClass(object):
        __slots__ = ['name', 'identifier']
        def __init__(self, name, identifier):
                self.name = name
                self.identifier = identifier

num = 1024*256
x = [MyClass(1,1) for i in range(num)]
In [3] used 0.2305 MiB RAM in 0.12s, peaked 0.00 MiB above current, total RAM usage 15.80 MiB

In [4]: from slots import *
In [4] used 9.3008 MiB RAM in 0.72s, peaked 0.00 MiB above current, total RAM usage 25.10 MiB

In [5]: %cat noslots.py
class MyClass(object):
        def __init__(self, name, identifier):
                self.name = name
                self.identifier = identifier

num = 1024*256
x = [MyClass(1,1) for i in range(num)]
In [5] used 0.1758 MiB RAM in 0.12s, peaked 0.00 MiB above current, total RAM usage 25.28 MiB

In [6]: from noslots import *
In [6] used 22.6680 MiB RAM in 0.80s, peaked 0.00 MiB above current, total RAM usage 47.95 MiB
```



