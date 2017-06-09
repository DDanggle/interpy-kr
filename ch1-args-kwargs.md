# 1. `*args` and `**kwargs`

많은 파이썬 프로그래머 입문자들이 \*args와 \*\*kwargs라는 멋진 변수에서 헤맵니다. 그래서 뭘까요? 먼저 꼭 `*args`와 `**kwargs` 라는 이름으로 사용할 필요는 없다고 알려드리고 싶습니다.  \*`별표`만 꼭 사용하면 됩니다. `*var`,  `**vars`로 사용하셔도 됩니다. `*args`와 `**kwargs`는 관례처럼 사용할 뿐입니다. \*args부터 먼저 살펴봅시다.

### 1.1 \*args의 사용법

대부분 \*args와 \*\*kwargs는 함수를 정의 할 때 사용됩니다. \*args와 \*\*kwargs는 가변 갯수의 인자들을 함수에 넣어줍니다. 여기서 가변 갯수의 인자라 함은, 사용자들이 얼마나 많은 인자들을 함수에 넣을지 모르는, 즉 갯수가 변할 수 있는 상황에서 \*args와 \*\*kwargs를 사용할 수 있다는 뜻입니다.  
\*args는 **키워드 되지않은** 가변 갯수의 인자들을 함수에 보낼 때 사용합니다. 좀 더 명확하게 예를 들어보겠습니다.

```py
def test_var_args(f_arg, *argv):
    print ("첫 번째 인자:", f_arg)
    for arg in argv:
        print ("*argv의 다른 인자", arg)

tast_var_args('야숩', 'python', '달걀', 'test')
```

다음과 같은 결과를 반환합니다.

```
첫번째 인자: 야숩
*argv의 다른 인자: python
*argv의 다른 인자: 달걀
*argv의 다른 인자: test
```

여러분들이 가지고 있던 헷갈린 부분이 해결되었으리라 생각합니다. 그러면 이제 \*\*kwargs에 대해서 알아봅시다.

### 1.2 \*\*kwargs의 사용법

\*\*kwargs는 **키워드된** 가변 갯수의 인자들을 함수에 보낼 때 사용합니다.\(역주:  가장 큰 차이는 keyword이냐 nonkeyword이냐입니다.\) \*\*kwargs는 함수가 **이름이 지정된 인자**를 처리할 때 사용해야합니다. 아래의 예시를 보겠습니다.

```py
# python2.7.9
def greet_me(**kwargs):
    if kwargs is not None:
        for key, value in kwargs.iteritems(): # python3: kwargs.items()
            print "%s == %s" % (key, value) # python3: print("%s == %s" % (key, value))

>>> greet_me(name="yasoob")
name == yasoob
```

함수에서 어떻게 키워드된 전달인자 리스트를  다룰 수 있는지 보았습니다. 위 예시는 단지 \*\*kwargs의 기본적인 방법일 뿐이고, 이것이 얼마나 유용한지 알게될 것입니다. 전달하고자 하는 인자들의 리스트나 사전\(dictionary\)을 가지고 함수를 호출하기 위해서 \*args, \*\*kwargs를 사용하는 방법을 알아보겠습니다.

### 1.3 함수를 호출하기 위한 \*args와 \*\*kwargs

\*args와 \*\*kwargs를 사용해서 어떻게 함수를 호출 알아보겠습니다. 아래의 간단한 함수만 생각해보겠습니다.

```py
def test_args_kwargs(arg1, arg2, arg3):
        print ("인자1:", arg1)
        print ("인자2:", arg2)
        print ("인자3:", arg3)
```

이제 위의 간단한 함수에게 인자를 전달하기 위해 \*args 혹은 \*\*kwargs를 사용할 수 있습니다. 아래와 같이 사용합니다:

```
# *args의 첫번째
>>> args = ("two", 3, 5)
>>> test_args_kwargs(*args)
인자1: two
인자2: 3
인자3: 5

# 이제 **kwargs:
>>> kwargs = {"인자3": 3, "인자2": "two", "인자1": 5} # arg(숫자)는 위 함수의 인자의 이름과 같아야합니다.
>>> test_args_kwargs(**kargs)
인자1: 5
인자2: two
인자3: 3
```

**\*arg, \*\*kwargs, 형식 인자\(format args\)의 사용순서**

위 3가지를 함수에서 동시에 사용하려면 아래와 같이 사용합니다.

```
some_func(fargs, *args, **kwargs)
```

### 1.4 언제 사용할까요?

목적에 따라 달라집니다. 일반적으로 함수의 데코레이터\(다른 챕터에서 이야기를 나눌 것입니다\)를 만들 때 사용합니다. 몽키패칭을 할 때도 사용할 수 있습니다. 몽키패칭은 런타임\(실행\) 중에 코드 일부를 수정한다는 것입니다. API를 호출하고 응답데이터를 반환하는 `get_info`라는 함수가 들어있는 클래스가 있다고 생각해보세요. API를 호출하고 어떤 테스트 데이터로 바꾸는 코드를 만든다면 아래와 깉이 할 수 있습니다.

```py
import someclass

def get_info(self, *args):
    return "Test Data"

someclass.get_info = get_info
```

이제 다른 상황에서도 잘 활용할 수 있을 것이라고 생각합니다.

