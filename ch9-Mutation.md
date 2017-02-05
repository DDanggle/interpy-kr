# 9. 변형

프로그래밍 초보자에게 파이썬의 변형가능한\(mutable\) 데이터타입과 불변형\(immutable\) 데이터타입은 지끈거리게 합니다. 간단하게 말하자면, 변형가능함\(mutable\)은 '변화가 가능하다는 것'이고 불변\(immutable\)은 '일관성 있다는 것'이라는 것입니다. 머리가 아프시죠? 아래의 예제를 봅시다.

```python
foo = ['hi']
print(foo)
#Output: ['hi']

bar = foo
bar += ['bye']
print(foo)
#Output: ['hi', 'bye']
```

무슨 일이 일어났나요? 이걸 기대하지는 않았을 겁니다! 아래 예제와 같은 것을 기대했을 것입니다.

```python
foo = ['hi']
print(foo)
#Output: ['hi']

bar = foo
bar += ['bye']

print(foo)
#Output: ['hi']

print(bar)
#Output: ['hi', 'bye']
```

위는 버그가 아닙니다. 실행되면서 변형된거죠. 어떤 변수를 다른 변형가능한 데이터타입 변수에 할당한다면, 두 변수에 따라 모두 데이터가 변화됩니다.  
새로운 변수는 오래된 변수로 바뀌어버립니다. 아래에 좀 더 알기 쉬운 함수와 함께 변환가능한 데이터 타입을 설명해보겠습니다

```python
def add_to(num, target=[]):
  target.append(num)
  return target

add_to(1)
# Output: [1]

add_to(2)
# Output: [1, 2]

add_to(3)
# Output: [1, 2, 3]
```

위와는 다르게 동작했을 것이라고 생각했을 것입니다. 아래와 같이 `add_to`를 호출할 때 마다 새로운 리스트가 만들어질 것이라 생각했을 것입니다.

```
def add_to(num, target=[]):
  target.append(num)
  return target

add_to(1)
# Output: [1]

add_to(2)
# Output: [2]

add_to(3)
# Output: [3]
```

리스트의 변환은 이렇게 짜증을 나게 하기도 합니다. 파이썬의 디폴트 인자들은 함수가 호출될 때마다가 아니라, 함수가 정의될 때 평가되어 생성됩니다. 무엇을 해야하는지 정확하게 알고 있다면, 처음부터 디폴트 인자를 변환형으로 절대 정의해놓으면 안됩니다. 아래와 같은 방법으로 해야 합니다.

```python
def add_to(element, target=None):
  if target is None:
      target = []
      target.append(element)
    return target
```

이제는 타겟 인자없이 함수를 호출하더라도 새로운 리스트가 생성됩니다. 예를 들어 보겠습니다.

```python
add_to(42)
#Output: [42]

add_to(42)
#Output: [42]

add_to(42)
#Output: [42]
```



