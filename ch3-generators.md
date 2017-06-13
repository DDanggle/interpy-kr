# 3. Generators

_\(역자 주: 이터레이터는 반복자등의 형태로 번역되어 쓰이지만, 이곳에서는 단원 등 명사로서 제시할 때는 반복자를, 문장에서는 이터레이터를 사용해서 이질감을 줄이고자 했습니다. \)_

먼저 이터레이터에 대해 이해해보겠습니다. 위키피디아에 따르면 이터레이터는 프로그래머가 컨테이너, 특히 목록을 탐색 할 수있게 해주는 객체입니다. 그러나 이터레이터는 순회를 수행하고 컨테이너의 데이터 요소에 대한 액세스를 제공하지만, 반복\(iteration\)을 수행하지는 않습니다. 무슨 말인지 복잡 해보이지만, 차근차근 알아보겠습니다. 즉, 세 부분에 대해서 알아보겠습니다.

* 반복가능한 객체 \(Iterable\)
* 반복자\(Iterator\)
* 반복\(Itreation\)

위 세 부분은 서로 연결되어 있습니다. 하나씩 먼저 설명하고 난 후에 제너레이터에 대해서 이야기 해보겠습니다.

### 3.1 반복가능한 객체 \(Iterable\)

반복가능\(`iterable`\)은 반복자를 반환하거나 인덱스를 취할 수있는 \_\_iter\_\_ 또는 \_\_getitem\_\_ 메소드가 정의 된 파이썬의 모든 객체입니다. \(두 메소드 모두 이전 챕터에서 자세히 설명했습니다.\) 간단히 말해서 반복가능\(**iterable**\)은 '**반복자'\(iterator\)'**를 제공 할 수 있는 모든 객체입니다. 그렇다면 반복자\(**iterator**\) 무엇일까요?

### 3.2 반복자\(Iterator\)

이터레이터는 `next`\(python2\) 혹은 `__next__`메소드가 정의된 모든 객체를 말하는 것입니다. 이것이 이터레이터입니다. 이제 그러면 **반복**에 대해서 이야기해보겠습니다.

### 3.3 반복\(Iteration\)

간단히 말하자면 리스트 같은 저장소에서 아이템을 가져오는 과정입니다. 만약 루프에서 루프로 어떤것을 옮기면 이것이 바로 이터레이션입니다. 과정 자체가 이터레이션인 것이죠. 이러한 용어들에 대한 기본적인 이해를 바탕으로 **제너레이터**를 이해해 보겠습니다.

### 3.4 Generators

제너레이터는 이터레이터지만 단 한 번만 반복합니다. 메모리에 모든 값을 저장하지 않기 때문에 값을 사용할 때 즉시 생성합니다. 'for'루프를 사용하거나 반복하는 함수나 구조에 생성된 값들을 전달하여 반복을 통해 사용합니다. 대부분의 제너레이터들은 함수로 구현 됩니다. 그러나, 값을 `반환`하지않고,  `산출`\(yield\)될 뿐입니다. 다음은 제너레이터 함수의 간단한 예제 입니다.

```py
def generator_function():
    for i in range(10):
        yield i

for item in generator_funtion():
    print(item)

# output: 0
# 1
# 2
# 3
# 4
# 5
# 6
# 7
# 8
# 9
```

위 경우는 그리 유용해 보이지 않습니다. 제너레이터는 모든 결과물들을 메모리에 저장하지 않으면서 동시에, 많은 양의 결과 셋을 계산해야할 때 최고입니다. \(특히 루프 그 자체를 포함한 계산을 할 때\). 파이썬 2에서리스트를 반환하는 많은 표준 라이브러리 함수들이 파이썬 3에서 생성자를 반환하도록 수정되었습니다. 왜냐하면 제너레이터가 더 적은 자원을 필요로 하기 때문입니다. 아래에 피보나치 수열을 계산하는 예시가 있습니다.

```py
# gerator version
def fibon(n):
    a = b = 1
    for i in xrange(n): # 역자: range(n)로 해야합니다.
        yield a
        a, b = b, a+b
```

아래와 같이 사용할 수 있습니다.

```py
for x in fibon(1000000):
    print (x)
```

자원을 많이 사용하는 것이 걱정이라면 위 방법에서는 걱정하지 않아도 됩니다. 그러나 아래와 같이 더 향상시킬 수도 있습니다.

```py
def fibon(n):
    a = b = 1
    result = []
    for i in xrange(n):
        result.append(a)
        a, b = b, a + b
    return result
```

매우 큰 입력값을 계산하도록 모든 자원을 사용하는 경우입니다 . `제너레이터`를 사용하면 오직 한번만 반복할 수 있다고 했습니다만, 확인 해보지는 않았습니다. 확인 해 보기 전에, 여러분들은 파이썬의 내장함수 `next()`대해서 알아야합니다. 이 함수는 시퀀스의 다음 요소에 접근할 수 있게 해 줍니다. 이제 확인 해보겠습니다.

```py
def generator_function():
    for i in range(3):
        yield i

gen = generator_function()
print(next(gen))
# output: 0
print(next(gen))
# output: 1
print(next(gen))
# output: 2
print(next(gen))
# output: Traceback (most recent call last):
#  File "<stdin>", line 1, in <module>
#  StopIteration
```

모든 변수가 산출되고 나서`next()`를 호출하면 `StopIteration` 에러가 뜬다는 것을 보았습니다.기본적으로 이 에러는 우리에게 값이 산출되었다고 알려주는 것입니다. 왜 `for` 루프를 사용할 때는 이러한 에러가 뜨지않는 지 궁금할 것입니다. 답은 간단합니다. `for` 루프는 자동으로 이 에러를 확인하고 `next`함수를 더 이상 호출하지 않습니다. 혹시 파이썬의 내장 데이터타입 몇몇은  반복\(iteration\)을 지원한다는 사실을 아시나요? 확인 해보겠습니다.

```py
my_string = "Yasoob"
next(my_string)
# Traceback (most recent call last):
#  File "<stdin>", line 1, in <module>
# TypeError: 'str' object is not an iterator
```

우리가 기대 했던 결과가 아닙니다.  에러는 `str` 가 이터레이터가 아니라고 합니다. 맞습니다! 반복가능\(iterable\)하지만 이터레이터는 아닙니다. 반복\(iteration\)은 지원하지만 직접 반복시킬 수는 없다는 거죠. 그렇다면 어떻게 이것을 반복시킬 수 있을까요? 이제 `iter`라는 파이썬 내장함수를 하나 더 배워보겠습니다. `iter`함수는 반복가능\(iterable\)으로부터 `이터레이터`객체를 반환합니다. 정수형은 반복가능하지않기 때문에, 문자열을 사용해야합니다!

```js
int_var = 1779
iter(int_var)
# Output: Traceback (most recent call last):
#   File "<stdin>", line 1, in <module>
# TypeError: 'int' object is not iterable
# This is because int is not iterable

my_string = "Yasoob"
my_iter = iter(my_string)
print(next(my_iter))
# Output: 'Y'
```

훨씬 좋아졌습니다. 제너레이터에 대해서 배우는 것을 좋아하리라 생각합니다. 이 개념을 사용할 때만 완전히 이해할 수 있다는 것을 명심하십시오. 이 방법을 따라해보고 적절한 상황에서 `제너레이터`를 사용해보세요. 절대 실망하지 않을 것입니다!

