파이썬은 콜렉션\(collections\)이라는 컨테이너 데이터 타입들이 포함된 모듈들을 제공합니다. 그 중 몇몇에 대해서 알아보고, 유용한 점에 대해서 이야기 해보겠습니다.

앞으로 이야기 해볼 것들은 다음과 같습니다.

* defaultdict
* counter
* deque
* namedtuple
* enum.Enum\(모듈 외부;파이썬 3.4 이상\)

### 12.1 `defaultdict`

개인적으로 defaultdict를 종종 사용합니다. dict와는 달리 defaultdict를 사용하면 key값의 존재 유무를 확인할 필요가 없습니다. 아래와 같이 할 수 있습니다.

```python
from collections import defaultdict

colours = (
  ('태희', '노랑'),
  ('지훈', '파랑'),
  ('별이', '초록'),
  ('지훈', '검정'),
  ('태희', '빨강'),
  ('샛별', '실버'),
)

favourite_colours = defaultdict(list)

for name, colour in colours:
  favourite_colours[name].append(colour)

print(favourite_colours)

# output
# defaultdict(<type 'list'>,
# {'별이' : ['초록'],
# '태희' : ['노랑', '빨강'],
# '샛별' : ['실버'],
# '지훈' : ['파랑', '검정']
# })
```

또 다른 중요한 사용방법은 사전\(dictionary\)안에 nested list\(리스트 안에 또 다른 리스트가 있는 형태\)를 추가하는 것입니다.  
키 값이 사전 안에 존재하지않는다면, KeyError가 발생할 것입니다. defaultdict는 이런 문제를 창의적인 방법으로 해결합니다. KeyError가 뜨는 예시를 보고나서 defaultdict를 사용한 해결방법을 보도록 하겠습니다.

##### Problem:

```python
some_dict = {}
some_dict['colours']['favourite'] = "노랑"
# Raises KeyError: 'colours'
```

##### Soloution:

```python
import collections
tree = lambda: collections.defaultdict(tree)
some_dict = tree()
some_dict['colours']['favourite'] = "노랑"
# 제대로 작동 합니다.
```

json.dumps를 사용해서 some\_dict를 출력할 수도 있습니다. 예시코드를 보여드리면,

```python
import json
print(json.dumps(some_dict))
# Output: {"colours": {"favourite": "노랑"}}
```

### 12.2 OrderedDict

OrderedDict는 처음 삽입 될 때의 순서대로 항목을 정렬 된 상태로 유지합니다.기존 키의 값을 덮어 쓰더라도 해당 키의 위치는 변경되지 않습니다.  
그러나 항목을 삭제했다가 다시 삽입하면 키가 사전 끝으로 이동합니다.

##### Problem:

```python
colours =  {"빨강" : 198, "녹색" : 170, "파랑" : 160}
for key, value in colours.items():
    print(key, value)
# Output:
#   녹색 170
#   파랑 160
#   빨강 198
# 예측할 수 없는 순서로 항목이 검색됩니다.
```

##### Soloution:

```python
from collections import OrderedDict

colours = OrderedDict([("빨강", 198), ("녹색", 170), ("파랑", 160)])
for key, value in colours.items():
    print(key, value)
# Output:
#   빨강 198
#   녹색 170
#   파랑 160
# 입력 순서가 유지 됩니다.
```

### 

### 12.3 `counter`

Counter는 특정 아이템의 개수를 세는 함수 입니다. 예를 들면 각자의 선호 색깔을 세는데 사용할 수 있습니다.

```python
from collections import defaultdict

colours = (
  ('태희', '노랑'),
  ('지훈', '파랑'),
  ('별이', '초록'),
  ('지훈', '검정'),
  ('태희', '빨강'),
  ('샛별', '실버'),
)

favs = Counter(name for name, colour in colours)
print(favs)

# Output: Counter({
# '태희' : 2
# '지훈' : 2
# '별이' : 1
# '샛별' : 1
# })
```

또 파일 안에서 가장 많이 반복되는 줄을 세는 데 사용할 수 도 있습니다. 예를 들면,

```python
with open('filename', 'rb') as f:
  line_count = Counter(f)
print (line_count)
```

### 12.4 `deque`

deque는 추가나 삭제가 양 쪽에서 가능한 double ended queue를 제공합니다. 먼저 collections 라이브러리로부터 deque 모듈을 임포트하면 됩니다.

```python
from collections import deque
```

이제 deque 객체를 인스턴스화 할 수 있습니다.

```
d = deque()
```

파이썬 리스트처럼 작동하고 비슷한 메소드를 제공합니다. 예를 들면 다음과 같습니다 :

```python
d = deque()
d.append('1')
d.append('2')
d.append('3')

print(len(d))
# Output: 3

print(d[0])
# Output: '1'

print(d[-1])
# Output: '3'
```

deque 양 쪽에서 뺴낼 수도\(pop\) 있습니다.

```python
d = deque([i for i in ranges(5)])
print(len(d))
# Output: 5

d.popleft()
# Output: 0

d.pop()
# Output: 4

print(d)
# Output: deque([1, 2, 3])
```

또한 deque가 가질 수 있는 아이템의 수에 제한을 둘 수도 있습니다. 이 방법을 사용하면 아이템 개수가 최대에 도달했을때  반대쪽 끝에서 아이템들이 튀어 나옵니다. 아래의 예시를 보면서 설명하는 것이 좀 더 쉬울 것 같습니다.

```python
d = deque(maxlen=30)
```

여러분이 30개 이상의 아이템을 넣으면, 왼쪽의 가장 앞에 있는 것이 튀어 나올 것입니다. 또 새로운 값으로 어떤 방향에서도 리스트를 확장시킬 수 있습니다.

```python
d = deque([1,2,3,4,5])
d.extendleft([0])
d.extend([6,7,8])
print(d)
# Output: deque([0, 1, 2, 3, 4, 5, 6, 7, 8])
```

### 12.5 `namedtuple`

튜플에 대해서는 잘 알 것입니다. 튜플은 기본적으로 값의 시퀀스를 쉼표로 구분하여 저장할 수 있는 불변\(immutable\) 리스트입니다. 리스트와 거의비슷하지만 몇가지 중요한 차이점이 있습니다. 주요한 점은 리스트와 달리, 튜플에 있는 항목을 **재 할당 할 수 없다**는 것입니다. 튜플 안의 값에 접근하기 위해서는 아래와 같이 정수 인덱스를 사용합니다.

```
man = ('알리', 30)
print(man[0])
# Output: 알리
```

그렇다면 namedtuples는 무엇 일까요? 간단한 작업을 위해 튜플을 편리한 컨테이너로 변환합니다. namedtuples을 사용하면 튜플 안의 값들에 접근하기 위해 정수 인덱스 값들을 사용할 필요가 없습니다. 사전형\(dictionaries\)과 비슷하다고 생각할 수도 있겠지만, 사전형과 달리 불변입니다.

```python
from collections import namedtuple

Animal = namedtuple('Animal', 'name age type')
perry = Animal(name="perry", age=31, type="cat")

print(perry)
# Output: Animal(name="perry", age=31, type="cat")

print(perry.name)
# Output: 'perry'
```

위에서 볼 수 있듯이 "."연산자를 사용하여 이름만으로 튜플의 멤버에 액세스 할 수 있음 을 알 수 있습니다. 좀 더 살펴보겠습니다. 네임튜플은 인자 두개를 필요로 합니다. 튜플의 `이름`과 튜플의 `필드 이름` 이죠. 위 예시에서 튜플의 `이름`은 'Animal'이고, 튜플의 `field_names`은 'name', 'age', 'cat'입니다. 네임튜플은 튜플들을 _자가문서화\(self-document\)_ 시킵니다. 예시 코드를 잘 보면 무슨 일이 일어나는지 쉽게 이해될 것입니다.  
튜플의 멤버에 액세스하기 위해 정수 인덱스를 사용할 필요가 없으므로 코드를 유지 관리하기가 더 쉽습니다. 게다가, _**"네임튜플" 인스턴스는 인스턴스당 사전들을 가지지 않으므로**_, 일반 튜플보다 더 가볍고 메모리 사용량을 줄일 수 있습니다. 그래서 사전형보다도 더 빠릅니다. 그러나 튜플이기 때문에 _**네임튜플의 속성들은 불변**임을_을 명심해야합니다. 아래와 같이 실행될 수 없습니다.

```python
from collections import namedtuple

Animal = namedtuple('Animal', 'name age type')
perry = Animal(name="perry", age=31, type="cat")
perry.age = 42

# Output: Traceback (most recent call last):ㅂ
#       File "", line 1, in
#     AttributeError: can't set attribute
```

코드를 자가문서화\(self-documeting\)할 때는 꼭 네임튜플을 사용해야합니다. 또 _일반적인 튜플과 하위호환\(backward compatible\)_됩니다.  
그래서 네임튜플은 기존 튜플처럼 정수 인덱스 값들을 사용할 수도 있습니다.

```python
from collections import namedtuple

Animal = namedtuple('Animal', 'name age type')
perry = Animal(name="perry", age=31, type="cat")
print(perry[0])
# Output: perry
```

마지막으로 네임튜플을 사전형으로 아래와 같이 변환할 수 있습니다.

```python
from collections import namedtuple

Animal = namedtuple('Animal', 'name age type')
perry = Animal(name="perry", age=31, type="cat")
print(perry._asdict())
# Output: OrderedDict([('name', 'Perry'), ('age', 31), ...
```

### 12.6 `enum.Enum` \(python 3.4+\)

또 다른 유용한 콜렉션으로는 enum 객체가 있습니다. 파이썬 3.4이상\(PyPI에서도 `enum34`라는 이름으로 존재합니다.\) 에서 `enum` 모듈 안에 존재합니다. 열거 \(열거 형\)는 기본적으로 다양한 물건을 정리하는 방법입니다.

마지막 예제의 Animal 네임튜플을 살펴보겠습니다. 이것은`type` 필드를 가지고 있습니다. 그러나 문제는 이 타입이 문자열\(string\)이라는 것입니다. 이는 몇가지 문제를 만들어낼 수도 있습니다. 사용자가 만약 shit키를 누르고 `Cat`이라고 입력하면 어떻게 될까요? `CAT`이나 `kitten`은 어떨까요? Enumeration은 문자열\(string\)을 사용하지 않음으로써 문제를 해결하도록 도와줍니다. 아래의 예시를 보겠습니다.

```python
from collections import namedtuple
from enum import Enum

class Species(Enum):
    cat = 1
    dog = 2
    horse = 3
    aardvark = 4
    butterfly = 5
    owl = 6
    platypus = 7
    dragon = 8
    unicorn = 9
    # The list goes on and on...

    # But we don't really care about age, so we can use an alias.
    kitten = 1
    puppy = 2

Animal = namedtuple('Animal', 'name age type')
perry = Animal(name="Perry", age=31, type=Species.cat)
dragon = Animal(name="Drogon", age=4, type=Species.dragon)
tom = Animal(name="Tom", age=75, type=Species.cat)
charlie = Animal(name="Charlie", age=2, type=Species.kitten)

# And now, some tests.
>>> charlie.type == tom.type
True
>>> charlie.type
<Species.cat: 1>
```

위 방법은 에러를 더 적게 발생시킵니다. 그리고 이름 타입에만 열거콜렉션을 사용하면 됩니다.

열거형 멤버에 접근하는 3가지 방법이 있습니다. 예를들어 아래 코드는 모두 cat에 대한 값을 얻습니다.

```python
Species(1)
Species['cat']
Species.cat
```

`collection` 모듈을 빠르게 훑어보았습니다. 공식문서를 꼭 읽어보시기를 바랍니다.

