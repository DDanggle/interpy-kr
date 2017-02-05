# 19. For - Else

어떤 프로그래밍 언에서도 `루프`는 필수요소입니다. 더욱이 `for` 루프는 파이썬에서는 중요한 부분입니다. 그러나 입문자들이 잘 모르는 몇 가지들도 있습니다. 하나씩 살펴보겠습니다. 

잘 알고 있는 것부터 시작해봅시다. for 루프를 아래와같이 사용한다는 것은 잘 알고 있습니다.

```python
fruits = ['apple', 'banana', 'mango']
for fruit in fruits:
    print fruit.capitalize()

# Output: Apple
# Banana
# Mango
```

위 예제는 for 루프의 매우 간단한 구조입니다. 그러면 파이썬의 `for` 루프의 특징들 중에서 좀 덜 알려진 것을 살펴보겠습니다.

### 19.1 `else` 문:

대부분 친숙하지는 않겠지만, `for` 루프는 `else`문을 가지고 있습니다. 그리고 `else`문은 루프가 보통 끝나고나면 실행됩니다. 이 의미는 루프가 어떤 break문도 만나지 않았다는 것입니다. 어디서 사용할지 이해하기에 매우 유용한 것이 있습니다. 제가 알고 있는 많은 것들을 알려드리겠습니다.

루프를 실행하고 각 아이템을 검색하는 것이 일반적인 구조입니다. 아이템을 찾고나면, 보통 `break`를 사용해서 루프를 빠져나옵니다. 루프를 종료시키는 두 가지 시나리오를 보겠습니다.  
하나는 아이템을 발견하면 `break`를 만나게 루프를 빠져나오는 것이고, 나머지 시나리오는 루프를 끝까지 도는 것입니다. 그러면 둘 중 어떤 것이 루프를 종료시키는 원인인지 궁금할 것입니다.   
확인하기 위해서 깃발을 걸어서 루프가 끝날 때까지 살펴볼 수도 있고 else 문을 사용할 수도 있습니다.

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

공식 문서에서 간단한 예시를 가져와보겠습니다.

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



