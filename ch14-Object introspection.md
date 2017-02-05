# 14. Object introspection

컴퓨터 프로그래밍에서 객체 탐구\(introspection\)는 프로그램이 실행\(런타임\)되는 동안 객체의 타입을 결정하는 능력을 의미합니다. 이는 파이썬의 강력한 점 중 하나입니다. 파이썬의 모든 것들은 객체이고 그 객체들을 찾아볼 수도 있죠. 파이썬은 이것들을 지원하는 몇 가지 내장 함수와 모듈들을 제공 해줍니다.

### 14.1 `dir`

여기서는 `dir`에 관해서 배워보고, 객체 탐구에서 어떻게 유용하게 사용할 수 있을 지 알아봅시다.

`dir`은 객체 탐구를 위해 가장 중요한 함수 중 하나입니다. 객체의 속성\(attribute\)의 리스트와 메소드를 반환합니다. 예를 들여보겠습니다.

```python
my_list = [1, 2, 3]
dir(my_list)

# ['__add__', '__class__', '__contains__', '__delattr__', '__delitem__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__gt__', '__hash__', '__iadd__', '__imul__', '__init__', '__iter__', '__le__', '__len__', '__lt__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__reversed__', '__rmul__', '__setattr__', '__setitem__', '__sizeof__', '__str__', '__subclasshook__', 'append', 'clear', 'copy', 'count', 'extend', 'index', 'insert', 'pop', 'remove', 'reverse', 'sort']
```

위 객체 탐구는 모든 메소드 이름을 리스트로 제공해 줍니다. 메소드 이름을 다시 불러올 필요가 없기 때문에 유용합니다. 만약 어떤 전달 인자도 없이 dir\(\)을 실행하면 현재 스코프내의 모든 이름들을 반환할 것입니다.

### 14.2 `type` 과 `id`

타입 함수는 함수의 타입을 반환합니다.

```python
print(type(􏰀􏰀''))
# Output: <type 􏰀str􏰀>

print(type([]))
# Output: <type 􏰀list􏰀>

print(type({}))
# Output: <type 􏰀dict􏰀>

print(type(bool))
# Output: <type 􏰀type􏰀>

print(type(3))
# Output: <type 􏰀int􏰀>
```

id는 다양한 객체의 유니크한 id를 반환합니다. 예를 들면,

```python
name = "yasoob"
print(id(name))
# Output: 139972439030304
```

### 14.3 inspect module

`inspect`모듈은 또한 살아있는 객체 정보를 알아내기 위한 유용한 함수들을 제공합니다. 예를 들면, 실행되고 있는 객체들의 요소들을 확인할 수 있습니다.

```python
import inspect
print(inspec.getmembers(str))
# Output: [(􏰀__add__􏰀, <slot wrapper 􏰀__add__􏰀 of ... ...
```

객체탐구 중에서 코드를 유용하게 만들어 줄 다양한 메소드들이 있습니다. 원하신다면 더 찾아보세요!

