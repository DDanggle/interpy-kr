# 20. 파이썬 C 확장

CPython이  개발자에게 제공하는 흥미로운 기능은 C 코드와 Python의 인터페이스 용이성 입니다.

개발자가 Python 코드에서 C 함수를 호출하는 데 사용하는 세 가지 주요 방법은  ctypes, SWIG 및 Python / C API가 있습니다. 각 방법마다 장점과 단점이 있습니다.

```python
fruits = ['apple', 'banana', 'mango']
for fruit in fruits:
    print fruit.capitalize()

# Output: Apple
#         Banana
#         Mango
```

위 예제는 for 루프의 아주 기본적인 구조입니다. 그러면 파이썬의 `for` 루프의 좀 덜 알려진 특성들을 살펴보겠습니다.

### 19.1 `else` 문:

대부분 친숙 하지는 않겠지만, `for` 루프는 또한 `else`문을 가지고 있습니다.  `else`문은 루프가 정상적으로 완료되면 실행됩니다. 이 의미는 루프가 어떤 break문도 만나지 않았다는 것입니다. 사용법을 이해하면 정말 유용합니다. 저는 정말 늦게 그것들을 알게 됐습니다.

루프를 실행하고 각 아이템을 검색하는 것이 일반적인 구조입니다. 아이템을 찾으면 `break`를 사용해서 루프를 빠져나옵니다. 루프를 종료시키는 두 가지 시나리오가 있습니다. 첫번째는 아이템을 발견하고 `break`를 만나 루프를 빠져나오는 것 입니다. 두번째 시나리오는 루프를 끝까지 도는 것입니다. 그러면 둘 중 어떤 것이 루프를 종료시키는 원인인지 궁금할 것입니다. 한 가지 방법은 플래그를 설정 한 다음 루프가 끝나면 플래그를 확인하는 것입니다. 다른 하나는 else 절을 ​​사용하는 것입니다.

`for/else` 루프의 기본 구조입니다.

```python
for item in container:
    if search_somting(item): 
        # 찾았습니다!
        process(item)
        break
else:
    # 아무것도 찾지 못했을 떄
    not_found_in_container()
```

공식 문서에서 가져온 간단한 예시를 보겠습니다.

```python
for n in range(2, 10):
    for x in range(2, n):
        if n % x == 0:
            print (n, '=', x, '*', n/x)
            break
```

위 결과값은 2와 10사이의 인수\(소수가 아닌수\)들입니다. 지금부터가 재미있는 부분입니다. `else`블록을 추가해서 소수들을 뽑아보겠습니다.

```python
for n in range(2, 10):
    for x in range(2, n);
        if n % x == 0:
            print (n, '=', x, '*', n/x)
            break
    else:
        # 루프가 끝나고 요소를 못 찾았을 때 
        print (n, '은 소수입니다.')
```



