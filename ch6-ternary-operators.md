# 6. Ternary Operators

삼항연산자는 파이썬에서 조건표현식\(conditional Expressions\)으로 잘 알고 있을 것입니다. 이 연산자는 조건문의 참-거짓에 따라 표현식을 평가합니다. 파이썬 2.4부터 지원되었습니다.

아래에 기본 코드 청사진과,  조건 표현식을 사용한 예시들을 소개합니다.

##### 코드의 청사진**:**

```py
condition_is_true if condition_ture else condition_is_false
```

**예시:**

```py
is_fat = True
state = "fat" if is_fat else "not fat"
```

if 조건문 몇 줄을 적는 것보다 훨씬 더 빨리 테스트할 수 있습니다. 유지가능한 코드를 만들어줄 뿐더러 코드를 간단하게 해 주기 때문에 자주 사용하면 정말 도움이 될 것입니다.

아직까지 덜 알려져있고 널리 사용되는 방법은 아니지만, 튜플을 포함시켜서 사용하는 다른 방법도 있습니다. 아래와 같이 사용합니다.

**코드의 청사진:**

```
(if_test_is_false, if_test_is_true)[test]
```

**예시:**

```py
fat = True
fitness = ("skinny", "fat")[fat]
print("Ali is ", fitness)
# Output: Ali is fat
```

True == 1이고 False == 0 이기 때문에 간단하게 동작하고, 리스트의 결과값을 튜플에 더할 수 있습니다.

위 예제와 같은 방법은 많이 사용되지는 않고 파이썬 사용자들이 파이썬스럽지않아 좋아하지 않습니다. 튜플 안에 어디에서 True값, False값을 넣어야 할지 혼동하기 쉽습니다.

또 if-else 삼항연산자에 반해, 튜플들의 요소 둘 모두 평가하기 때문에 튜플 삼항 연산자 사용을 피하는 것이 좋습니다.

**예시:**

```py
condition = True
print(2 if condition else 1/0)
#Output is 2

print((1/0, 2)[condition])
#ZeroDivisionError is raised
```

튜플 삼항 테크닉이기 때문에 먼저 튜플을 만들고 인덱스를 찾습니다. if-else 삼항연산자는 일반적인 if-else 로직을 따라갑니다. 그러므로, 조건에 따라 에외를 발생시켜야한다거나 무거운 계산이 필요한 경우에는 사용하지 않는 것이 좋습니다.

