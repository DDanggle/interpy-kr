# 8. Global & Return

함수의 마지막 반환 값이 키워드값인 파이썬 함수를 본 적이 있을 것입니다.   
이 함수는 어떤 역할을 하는 지 아시나요? 이것은 다른 프로그래밍 언어에서의 반환과 비슷합니다. 간단한 함수로 확인 해보겠습니다.

```python
def add(value1, value2):
  return value1 + value2

result = add(3,5)
print(result)
#Output: 8
```

위 함수는 입력값 두 개를 더해서 나온 결과값을 보여줍니다. 아래와 같이 할 수도 있습니다. 

```python
def add(value1, value2):
  global result
  result = value1 + value2

add(3,5)
print(result)
# Output: 8
```

키워드 return이 포함된 첫번째 코드부터 살펴봅시다. 이 함수는 `result`라는 함수를 호출하면서, 값을 변수에 할당하는 일을 합니다. 대부분에는 global 키워드를 사용할 필요는 없습니다. 하지만 global 키워드가 포함된 아래의 코드를 살펴봅시다. 이 함수는 실제로는 전역변수 result 값을 만드는 일을 합니다. 여기서 global\(전역\)의 의미는 무엇일까요? 전역변수는 이 함수의 스코프 밖에서도 이 변수에 접근이 가능하다는 것을 의미합니다. 아래와 같은 예시를 통해서 설명 해보겠습니다.

```python
# 먼저 전역변수 없이 사용 해 보겠습니다.
def add(value1, value2):
  result = value1 + value2

add(2,4)
print(result)

# 여기서 예외처리를 맞닥뜨렸네요! 왜 그럴까요?
# 파이썬 인터프리터는 우리가 result라 부르는 변수를 가지고 있지 않다고 말하네요.
# 왜냐하면 result 변수는 변수가 생성된 함수 안에서 유효하기 때문에, global이 아니라면 접근할 수 없습니다.
Traceback (most recent call last):
  File "<console>", line 1, in <module>
NameError: name 'result' is not defined

# result를 만들고 난 후, 동일한 코드를 실행 해보겠습니다.
# 전역변수 (variable) global

def add(value1, value2):
  global result
  result = value1 + value2

 add(2,4)
 result
 6
```

두 번째 실행에서는 예상한 대로 에러가 뜨지않습니다. 프로그래밍 입문단계에서는 전역범위에서 원하지 않는 변수가 튀어나와 갑작스러운 문제에 부닥칠 수 있어서, global키워드를 사용에 조심 해야합니다.

### 8.1 다수의 반환값들

만약 한 개의 변수가 아닌 두 변수를 반환하고 싶으면 어떻게 할까요? 프로그래밍 초보들이 택할 몇가지 방법들이 있습니다. 가장 인기있는 접근방법은 `global`키워드를 사용합니다. 의미는 없는 간단한 예제를 살펴봅시다.

```py
def profile():
    global name 
    global age
    name = "Danny"

profile()
print(name)
# Output: Danny

print(age)
# Output: 30
```

**Note:** 위에서 사용한 방법으로 사용하지 마십시요, 다시 한 번 말하지만, 위에 있는 방법으로 사용하지 마십시요. 

함수가 끝날 때 필요한 변수들을  `tuple`, `list`, `dict` 형태를 반환함으로써 이런 문제들을 풀 수도 있습니다.

```python
def profile():
    name = "태희"
    age = 30
    return (name, age)    

profile_data = profile()
print(profile_data[0])
# Output: 태희

print(profile_data[1])
# Output: 30
```

혹은 좀 더 일반적인 방법으로는,

```python
def profile():
    name = "태희"
    age = 30
    return name, age

profile_name, profile_age = profile()
print(profile_name)
# Output: 태희
print(profile_age)
# Output: 30
```

`lists`나 `dict`를 반환하는 것보다 좀 더 나은 방법입니다. `global` 키워드가 어떤 역할을 하는 지 정확히 알지 않은 한 사용하지 마세요. `global` 키워드가 유용할 때도 있지만, 대부분은 아닙니다.

