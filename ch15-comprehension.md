# 15. comprehension

*\(역주: 컴프리헨션은 `내포`로써도 많이 번역되지만, 어감과 느낌을 고려하여 컴프리헨션 그대로 번역하였습니다.\)*

컴프리헨션은 파이썬을 쓰지않는 날이 온다면 무척 그리워하게될 파이썬의 특징입니다. 컴프리헨션은 다른 구성 단위\(sequence\)를 사용해서 새로운 구성 단위\(sequence\)를 만듭니다. 컴프리헨션에는 세 가지 종류가 있습니다.

* 리스트 컴프리헨션\(list comprehension\)
* 딕셔너리 컴프리헨션\(dictionary comprehension\)
* 셋 컴프리헨션\(set comprehension\)

리스트 컨프리헨션\(list comprehension\)은 파이썬 2에서 소개되었고, set과 dictionary 컨프리헨션\(comprehension\)는 파이썬 3.0 버전부터 소개되었습니다.

하나씩 살펴보도록 하겠습니다. 여러분이 리스트 컴프리헨션을 어느정도 이해한다면, 다른 컴프리헨션들도 쉽게 사용할 수 있을 것입니다.

### 15.1 list comprehensions

리스트 컴프리헨션은 리스트를 만드는 짧고 간결한 방법을 제공합니다. \[\] 안에 표현식과 for문이 나오고, 0 혹은 다른 for이나 if문이 추가됩니다.  
이 표현식안에는 말 그대로 어떤 종류의 객체도 리스트에 넣을 수 있습니다. if문 혹은 for문으로 구성된 표현식이 실행되고 난 후의 값들로 새로운 리스트가 생성됩니다.

##### 소스 코드의 청사진:

```python
variable = [out_exp for out_exp in input_list if out_exp == 2]
```

간단한 예시부터 보겠습니다.

```python
multiples = [i for i in range(30) if i % 3 is 0]
print(multiples)
# Output: [0, 3, 6, 9, 12, 15, 18, 21, 24, 27]
```

정말 쉽고 빠르게 리스트를 만들 수 있습니다. 어떤 상황에서는 `filter`함수를 쓰는 것보다 더 좋습니다. 리스트 컴프리헨션은 for 루프안에서 반복하는 동안 함수나 메소드등을 실행시켜서 새로운 리스트를 만들고자 할 때 빛을 발합니다. 예를 들면 이런 식으로 할 수 있습니다.

```python
squared = []
for x in range(10):
  squared.append(x**2)
```

를 리스트 컴프리헨션을 사용하면 아래와 같이 간단하게 만들 수 있습니다.

```python
squared = [x**2 for i in range(10)]
```

### 15.2 dict comprehension

비슷한 방법으로 사용할 수 있습니다. 제가 최근에 찾은 예시를 보겠습니다.

```python
mcase = {'a': 10, 'b': 34, 'A':7, 'Z':3}

mcase_frequency = { k.lower() : mcase.get(k.lower(), 0) + mcase.get(k.upper(), 0) for k in mcase.keys() }
# mcase_frequency == {'a': 17, 'z': 3, 'b': 34}
```

위 예제와 같이 다른 타입의 경우에서도 같은 방식으로 키 값들을 결합시킬 수 있습니다. 저는 개인적으로 딕트 컴프리헨션을 많이 사용하지는 않습니다. 아래와 같이 사전\(dictionary\)을 쉽고 빠르게 뒤집을 수도 있습니다.

```python
{v: k for k, v in some_dict.items() }
```

### 15.3 set comprehensions

셋 컴프리헨션도 리스트 컴프리헨션과 비슷합니다. 괄호 `{}` 를 사용하는 것과 반환값이 제너레이터인 것만 다릅니다.

```python
squared = (x**2 for i in range(10))
print(squared)
#Output: <generator object <genexpr> at 0x00000000029931B0>
squared.next()
# Output: 0
```



