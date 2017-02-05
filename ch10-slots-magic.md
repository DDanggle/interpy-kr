# 10. __slots__Magic

파이썬에서 모든 클래스들은 인스턴스 속성을 가질 수 있습니다. 순수 파이썬에서는 객체의 인스턴스 속성을 저장하기위해 dict형을 사용합니다.
프로그램 실행 중에 임의적인 새로운 속성들을 설정하는 데 매우 도움이 됩니다. 

그렇지만, 알려진 속성들을 가지고 있는 작은 클래스에서는 문제가 될 수 있습니다. dict형은 램 메모리를 많이 낭비합니다. 파이썬은 객체 생성시에 모든 속성들을 저장하기 위해 메모리에 고정된 양을 할당할 수 없습니다.
그래서 많은 클래스를 만들면(수천개에서 수백만개), 램이 터질 수 있습니다. 아직까지도 이 문제는 해결되지 못했습니다.
__slots__을 사용하는 것을 포함시킴으로 파이썬 dict형을 사용하지 않도록 할 수 있습니다. 여기 __slot__없이 활용된 예시를 살펴보겠습니다.

Without __slots__:

```python
class MyClass(object):
    def __init__(name, class):
        self.name = name
        self.class = class
        self.set_up()
    # ...
```

With __slots__:

```python
class MyClass(object):
    __slots__ = ['name', 'class']

    def __init__(name, class):
        self.name = name
        self.class = class
        self.set_up()
    # ...
```

두 번째 코드는 램이 소모되는 것을 줄일 수 있습니다. 어떤 사람은 이 방법을 사용해서 램 사용량을 거의 40에서 50% 정도로 줄일 수 있다고 합니다.

다른 방법으로는 PyPy를 시도해보는 것도 좋습니다. 여기 말했던 모든 것이 최적화되어 기본적으로 들어가 있습니다.
