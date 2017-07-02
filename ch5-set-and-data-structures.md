# 5. `set` Data Structure

`set` 함수는 매우 유용한 데이터 구조입니다. `set` 함수는 같은 값을 포함하지 않도록 분별된 리스트처럼 동작합니다. 이것은 정말 많은 경우에 유용합니다. 예를 들어, 값이 중복으로 리스트에 존재하는지 확인하고 싶으면 두 가지 방법으로 확인할 수 있습니다. 하나는 `for` 루프를 사용하는 것이고, 아래와 같이 사용합니다

```py
some_list = ['a', 'b', 'c', 'b', 'd', 'm', 'n', 'n']

duplicates = []

for value in some_list:
    if some_list.count(value) > 1:
        if value not in duplicates:
            duplicates.append(value)

print(duplicates)
# output : ['b', 'n']
```

그렇지만 `set`을 사용하면 더 간단하고 우아한 코드가 완성됩니다. 아래와 같이 간단하게 만들 수 있습니다.

```py
some_list = ['a', 'b', 'c', 'b', 'd', 'm', 'n', 'n']

duplicates = set([x for x in some_list if some_list.count(x) > 1])
print(duplicates)

#output: set(['b', 'n'])
```

Set은 다른 메소드들도 가지고 있습니다. 몇 가지를 소개해드리겠습니다.

**Intersection\(교차\)**

두 셋 에서 중복된 것을 뽑아낼 수도 있습니다. 예를 들면:

```py
valid = set(['yellow', 'red', 'blue', 'green', 'black'])
input_set = set(['red', 'brown'])
print(input_set.intersection(valid))
#output: set(['red'])
```

##### Difference\(다름\)

위 예시에서 difference메소드를 사용해서 다른 값들을 뽑아낼 수 있습니다. 예를 들면,

```py
valid = set(['yellow', 'red', 'blue', 'green', 'black'])
input_set = set(['red', 'brown'])
print(input_set.difference(valid))
#Output: set(['brown'])
```

또한 { }를 사용해서 새로운 셋을 생성할 수도 있습니다.

```py
a_set = {'red', 'blue', 'green'}
print(type(a_set))

#Output: <type 'set'>
```

이것 말고도 몇가지 다른 메소드들도 있습니다. 공식문서를 방문해서 빠르게 쭉 읽어보시길 추천합니다.

