# 22. Coroutines

코루틴은 제너레이터와 몇가지를 제외하고는 거의 비슷합니다. 주된 다른점은,

* 제너레이터는 데이터의 생산자이고,
* 코루틴은 데이터의 소비자입니다.

먼저 제너레이터 만드는 과정을 돌이켜봅시다. 아래와 같이 제너레이터를 만들 수 있습니다.

```python
def fib():
    a, b = 0, 1
    while True:
        yield a, b = b, a + b
```

그 이후에 `for` 루프는 아래와 같이 사용합니다.

```python
for i in fib():
    print(i)
```

리스트에 값을 저장하는 게 아니라 `생산하고` 날려버리기 때문에 빠른데다 메모리에 부하를 주지도 않습니다.  우리가 위 예시에 `yield`를 사용하면 일반적으로 코루틴이 됩니다. 코루틴은 보내는 값들을 사용합니다. 아주 기본적인 예시인 `grep`을 사용하는 방법을 알아보겠습니다.

```python
def grep(pattern):
    print ("Searching for %s" % pattern)
    while True:
        line = (yield)
        if pattern in line:
            print(line)
```

잠깐! `yield`의 반환값은 무엇일까요?? 코루틴으로 반환값을 넣었습니다. 값을 처음부터 포함시킬 수 없고, 외부에서 값\(value\)을 포함시켜야합니다. `.send()` 메소드를 사용해서 값을 포함시킵니다.  
예제를 보겠습니다.

```python
search = grep('coroutine')
next(search)
# Output: Searching for coroutine
search.send("너를 사랑해")
search.send("너는 나를 사랑하지않니?")
search.send("그럼 나는 코루틴을 사랑할테야!")
# Output: 그럼 나는 코루틴을 사랑할테야!
```

보내진 값들은 `yield`가 접근합니다. 코루틴으로 시작하기 위해서는 `next()`를 꼭 실행주어야합니다. 제너레이터와 동일하게, 코루틴도 함수를 바로 시작할 수는 없습니다. 대신 응답에서 `__next__()`와 `send()` 메소드를 사용해서 실행합니다. 그러므로, `yield`문에서 실행될 수 있도록 `next()`를 실행 해 주어야합니다.

`.close()` 메소드를 사용해서 코루틴을 닫을 수도 있습니다.

```python
search = grep('coroutine')
#...
search.close()
```

이외에도 코루틴에 대한 정보는 많습니다. David Beazley의 [멋진 자료](http://www.dabeaz.com/coroutines/Coroutines.pdf)를 보시기를 추천합니다.

