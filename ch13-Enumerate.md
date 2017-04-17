# 13. Enumerate

Enumerate\(열거\)은 파이썬의 내장함수 입니다. 열거형의 유용성은 한줄로 요약할 수 없습니다. 그러나 대부분의 파이썬 입문자들나 심지어 중고급 개발자들도 잘 모르는 함수입니다. 그것은 우리가 무언가를 반복하고 자동 카운터를 가질 수있게 해줍니다. 예시를 보도록 하겠습니다.

```python
for counter, value in enumerate(some_list):
  print(counter, value)
```

이게 전부가 아닙니다. `enumerate` 은 선택적 인자\(optional argument\)들을 허용함으로써 더 유용하게 사용할 수 있습니다.

```python
my_list = ['사과', '바나나', '포도', '배']
for c, value in enumerate(my_list, 1):
  print (c, value)

# Output:
# 1 사과
# 2 바나나
# 3 포도
# 4 배
```

선택적 인자들은 `enumerate` 에 어떤 인덱스 값부터 시작해서 열거되는지 알려줍니다. 또 인덱스를 포함하는 튜플을 만들 수도 있고, 리스트를 사용해서 리스트 아이템을 만들 수 있습니다. 예를 들어보겠습니다.

```python
my_list = ['사과', '바나나', '포도', '배']
counter_list = list(enumerate(my_list, 1))
print(counter_list)

# Output: [(1, '사과'), (2, '바나나'), (3, '포도'), (4, '배')]
```



