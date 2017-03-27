# 4. Map, Filter and Reduce

프로그래밍에서 함수적 접근을 쉽게 해주는 세 가지 함수 들입니다. 하나씩 보면서 언제 사용할 지 생각해보겠습니다.

### 4.1 Map

`Map`은 입력 리스트들의 모든 항목에 함수를 적용합니다. 아래 일반적인 코드를 보겠습니다.

##### 코드의 청사진

```py
map(functions_to_apply, list_of_inputs)
```

대부분의 경우 리스트에 있는 모든 요소들을 함수에 전달하고 결과값들을 취합 해서 사용하려고 합니다. 예를 들면,

```py
items = [1, 2, 3, 4, 5]
squared = []
for i in items:
    squared.append(i**2)
```

`Map`을 사용하면 훨씬 간단하고 멋진 방식으로 구현할 수 있습니다. 다음 코드에 있습니다.

```py
items = [1, 2, 3, 4, 5]
squared = map(lambda x : x**2, items)
```

대부분 lambda\(익명함수\)와 map 함수를 함께 사용하기 떄문에 동일한 방식으로 코드를 작성했습니다. 리스트 형식의 입력값 대신에 함수들의 리스트를 사용할 수도 있습니다!

```py
def multiply(x):
    return (x*x)

def add(x):
    return (x+x)

funcs = [multiply, add]
for i in range(5):
    value = map(lambda x: x(i), funcs)

# output:
# [0,0]
# [1,2]
# [4,4]
# [9,6]
# [16,8]
```

### 4.2 Filter

'필터'라는 이름이 이야기해주듯, `필터`는 함수에서 참\(True\)를 반환하는 요소들로 구성되는 리스트를 생성합니다. 짧고 간단한 예시를 보겠습니다.

```py
number_list = range(-5, 5)
less_than_zero = list(filter(lambda x: x<0, number_list))
print(less_than_zero)

#output = [-5, -4, -3, -2, -1]
```

필터함수는 for loop과 비슷해보이지만, 내장 함수이고 속도도 더 빠릅니다.

Note: 맵과 필터가 아직 엄청나보이지 않는다면, `list/dict/tuple` 컴프리헨션에 대해 더 읽어보시면 좋겠습니다.

### 4.3 Reduce

`Reduce`는 리스트로 몇 가지 계산을 수행하고 반환하는 매우 유용한 함수입니다. 예를 들어 정수 리스트의 곱을 계산하려고 한다고 해봅시다. 

파이썬에서 이 작업을 수행하는 일반적인 방법은 for루프를 사용하는 것입니다.

이제 reduce를 사용해서 코드를 작성해 보겠습니다.

```py
from functools import reduce 
product = reduce((lambda x, y: x * y), [1, 2, 3, 4])

# Output: 24
```



