# 7. Decorators

데코레이터는 파이썬에서 매우 중요합니다.. 간단하게 말하면 다른 함수의 기능을 조작하는 함수입니다. 코드를 간결하고 더 파이썬스럽게 만들 있도록 도와줍니다. 대부분 입문자들은 언제 사용해야할 잘 몰라하는데,  데코레이터를 사용해서 코드를 간결하게 만들 수 있는 방법들을 공유 해보겠습니다.

먼저, 여러분 만의 데코레이터를 만드는 법에 대해 이야기 해봅시다.

완벽하기에 이해하기에 가장 어려운 개념 중 하나입니다. 그렇지만, 한 번에 한 단계씩 본다면 완전히 이해할 수 있을 것입니다.

### 7.1 파이썬의 모든 것들은 객체입니다:

첫 번째로 파이썬 안에서의 함수를 이해 해봅시다.

```python
def hi(name="yasoob"):
  return "hi " + name

print hi()
#output: 'hi yasoob'

# 변수에 함수를 저장합니다.
greet = hi
# 우린 함수 hi를 호출하지 않을 것이기 때문에 ()를 사용하지 않아도 됩니다.
# 대신 우리는 greet 라는 변수에 hi를 넣을 것입니다. 이것을 실행 해봅시다.

print greet()
#output: 'hi yasoob'

#만약 이전의 hi 함수를 지워버리면 어떻게 되는지 봅시다!
del hi
print hi()
#outputs: NameError

print greet()
#outputs: 'hi yasoob'
```

### 7.2 함수를 이용해서 함수를 정의해봅시다:

기본적인 함수가 오는 시기에 대해 살펴보았습니다. 이제 한 단계 더 나아가봅시다. 파이썬에서는 다른 함수에서 함수를 정의 할 수 있습니다.

```python
def hi(name="greet"):
  print "여러분은 hi() 함수 안에 있습니다"

  def greet():
    return "여러분은 greet() 함수 안에 있죠"

  def welcome():
    return "여러분은 welcome()함수 안에 있습니다"

  print greet()
  print welcome()
  print "이제 여러분은 다시 hi()함수에 있습니다."

hi()
# output: 여러분은 hi() 함수 안에 있습니다
#     여러분은 greet() 함수 안에 있습니다
#     여러분은 welcome()함수 안에 있습니다
#     이제 여러분은 다시 hi()함수에 있습니다.

# 위 예제는 여러분이 hi(), greet() 그리고 welcome()을 호출했을 때의 모습을
# 보여줍니다
# 그러나, greet(), welcome() 함수는 hi()함수 밖에서는 접근할 수 없습니다.

greet()
#output: NameError: name 'greet' is not defined
```

이제 함수 안에서 다른 함수를 정의할 수 있다는 것을 알았습니다. 간단하게 말하면 중첩 함수를 만들 수 있다는 것 입니다.이제 함수들이 함수를 반환할 수도 있다는 것을 배워보죠.

### 7.3 within 함수로부터 함수 리턴하기:

꼭 다른 함수와 함께 함수를 실행할 필요는 없지만, 우리는 결과값들을 다음과 같이 반환할 수 있습니다.

```python
def hi(name="yasoob")
  def greet():
    return "우리는 이제 greet()함수 안에 있습니다"

  def welcome():
    return "우리는 이제 welcome()함수 안에 있구요"

  if name == "yasoob":
    return greet
  else:
    return welcome
a = hi()
print a
# outputs: <function greet at 0x7f2143c01500>

# 이것은 확실히 'a'가 hi()함수안의 greet()함수를 가르키고 있다는 것을 보여줍니다.
# 이제 아래 것을 시도해봅시다.

print a()
# outputs: 우리는 이제 greet()함수 안에 있습니다.
```

코드를 다시 한 번 확인해보세요. if/else 조건문들이 `greet()`나 `welcome()`이 아니라 `greet`와 `welcome`을 반환합니다. 왜 그럴까요? 여러분이 \(\)를 넣었기 때문에 함수가 실행되었습니다. 만약 여러분이 \(\)를 넣지 않는다면, 그냥 지날 것이며 실행되지않고 다른 변수에 할당될 것입니다.  
이해하셨나요? 좀 더 자세히 설명해보죠. 우리가 `a = hi()`라고 썼을 때, `hi()`가 실행되고 name값이 yasoob으로 고정되어 있었기 때문에 함수 `greet`이 리턴되었습니다. 만약 여러분이 `a = hi(name= "ali")`라고 입력했다면, `welcome `함수가 리턴되었을 것입니다. 또한 `hi()()`를 출력하면 greet함수 안에서 결과값이 나올 것입니다.

### 7.4 다른 함수에서 인자로서 함수.

```python
def hi():
  return "hi yasoob"

def doSomethingBeforeHi(func):
  print "hi()가 실행되기 전에 좀 지루한 일을 먼저 해야합니다"
  print func()

doSomethingsBeforeHi(hi)
#output: hi()가 실행되기 전에 좀 지루한 일을 먼저 해야합니다
#    hi yasoob
```

이제 데코레이터를 배우기 위한 기본적인 지식들을 배웠습니다. 데코레이터는 함수 이전과 이후에 실행되게 해줄 것입니다.

### 7.5 첫번째 데코레이터 작성하기:

마지막 예제\(7.4\)에서 사실, 우리는 데코레이터를 만들었습니다! 이전의 데코레이터를 좀 수정하고 더 유용한 프로그램으로 만들어봅시다.

```python
def a_new_decorator(a_func):
  def wrapTheFunction():
    print "a_function_requiring_decoration()가 실행되기 전에 좀 지루한 일을 먼저 해야합니다"

    a_func()

    print "a_function_requiring_decoration()가 실행된 후에 좀 지루한 일을 해야합니다"
  return wrapTheFunction()

def a_function_requiring_decoration():
  print "저는 고약한 냄새를 없애기 위해 뭔가 추가된 함수입니다"

a_function_requiring_decoration()
#outputs: "저는 고약한 냄새를 없애기위해 뭔가 추가된 함수입니다"

a_function_requiring_decoration = a_new_decorator(a_function_requiring_decoration)
#이제 wrapTheFunction()에 a_function_requiring_decoration이 들어갔습니다.

a_function_requiring_decoration()
#outputs: "a_function_requiring_decoration()가 실행되기 전에 좀 지루한 일을 먼저 해야합니다"
#       "저는 고약한 냄새를 없애기위해 뭔가 추가된 함수입니다"
#       "a_function_requiring_decoration()가 실행된 후에 좀 지루한 일을 해야합니다"
```

이제 뭔가 감이 좀 오시나요? 작동원리에 대해서 배워봤습니다. 이게 정확히 파이썬에서 데코레이터가 하는 역할입니다. 함수를 감싼 후에 하나 혹은 다른 방법으로 함수에서 일어나는 행동들을 수정합니다.지금 쯤이면 왜 아직까지 어떤 코드에서도 @를 사용하지 않았는지 궁금하실 겁니다. @는 데코레이트된 함수를 만드는 가장 간결한 방법일 뿐입니다. 이전의 코드 예시를 @를 사용해서 실행해봅시다.

```python
@a_new_decorator
def a_function_requiring_decoration():
  """hey! yo! 데코레이트 해주세요!! """
  print "저는 고약한 냄새를 없애기위해 뭔가 추가된 함수입니다"

a_function_requiring_decoration()
#outputs: "a_function_requiring_decoration()가 실행되기 전에 좀 지루한 일을 먼저 해야합니다"
#       "저는 고약한 냄새를 없애기위해 뭔가 추가된 함수입니다"
#       "a_function_requiring_decoration()가 실행된 후에 좀 지루한 일을 해야합니다"

# @a_new_decorator은 위와 같은 방법을 아래와 같이 간단하게 줄인 것입니다.
a_function_requiring_decoration = a_new_decorator(a_function_requiring_decoration)
```

이제 파이썬에서 데코레이터가 기본적으로 어떻게 동작하는지 이해했으면 좋겠습니다. 위 코드에서 문제가 하나 있습니다. 만약 아래와 같이 실행하면,

```python
print(a_function_requiring_decoration.__name__)
#output: wrapTheFunction
```

이건 원하는 답이 아닙니다! 이 함수의 이름은 "a\_function\_requiring\_decoration" 이어야 합니다. 함수 이름이 wrapTheFunction으로 바뀌어 있습니다. 이는 이름을 오버라이딩시켜 함수에 독스트링\(코멘트\) 되었기 때문에 생긴 것입니다. 운이 좋게도 파이썬은 이 문제를 해결할 수 있는 `functools.wrap` 이라는 간단한 함수를 제공합니다.  
이전의 예제를 `functools.wrap`를 사용해서 수정해보겠습니다.

```python
from functools import wraps

def a_new_decorator(a_func):
  @wraps(a_func)
  def wrapTheFunction():
    print "a_function_requiring_decoration()가 실행되기 전에 좀 지루한 일을 먼저 해야합니다"
    a_func()
    print "a_function_requiring_decoration()가 실행된 후에 좀 지루한 일을 해야합니다"
  return wrapTheFunction

@a_new_decorator
def a_function_requiring_decoration():
  """자! 여기! 데코레이트 해주세요!! """
  print "저는 고약한 냄새를 없애기 위해 뭔가 추가된 함수입니다"

print(a_function_requiring_decoration.__name__)
#output: a_function_requiring_decoration
```

훨씬 좋아졌네요. 이제 데코레이터의 실제로 사용하는 방법에 대해서 배워보겠습니다.

**소스 코드의 청사진:**

```python
from functools import wraps
def decorator_name(f):
  @wraps(f)
  def decorated(*args, **kwargs):
    if not can_run:
      return "함수는 실행되지 않습니다."
    return f(*args, **kwargs)
  return decoraated

@decorator_name
def func():
  print "함수가 실행 중입니다."

can_run = True
print(func())
#output: 함수가 실행 중입니다

can_run = False
print(func())
#output: 함수는 실행 되지 않습니다.
```

Note : `@wraps`는 함수를 장식하고 함수 이름, 독스트링, 인자 리스트 등을 기능에 복사하여 추가합니다. 이는 데코레이터안에서 데코레이터가 장식되기 이전의 함수 속성에 접근할 수 있도록 해 줍니다.

### 7.5 사용 방법

데코레이터를 사용했을 때 가장 빛을 발하는 곳이 어디인지와 이런 사용이 관리를 얼마나 용이하게 해주는 지 알아봅시다.

### 7.6 인증

데코레이터는 웹 애플리케이션 내에 엔드포인트에서 사용자가 인증요청시 확인할 수 있도록 도와줍니다. 플라스크과 장고 웹프레임워크에서 널리 쓰이는 방법입니다. 아래에 데코레이터를 사용해서 만든 기본적인 인증에 대해 예를 들어보겠습니다.

```python
from functools import wraps

def requires_auth(f):
  @wraps(f)
  def decorated(*args, **kwargs):
    auth = request.authorization
    if not auth or not check_auth(auth.username, auth.password):
      return authenticate()
    return f(*args, **kwargs)
  return decorated
```

### 7.7 Logging

로깅에서도 데코레이터가 빛을 발하는 영역입니다. 예를 들면,

```python
from functools import wraps

def logit(func):
  @wraps(func)
  def with_logging(*args, **kwargs):
    print func.__name__ + " was called"
    return func(*args, **kwargs)
  return with_logging

@logit
def addition_func(x):
  """어떤 덧셈"""
  return x + x

result = addition_func(5)
# Output: addtion_func was called
```

지금쯤이면 데코레이터를 유용하게 사용할 곳들이 떠오를거라 확신합니다.

