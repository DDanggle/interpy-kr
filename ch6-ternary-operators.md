# 6. Ternary Operators

삼항연산자는 파이썬에서 조건표현식\(conditional Expressions\)으로 잘 알고 있을 것입니다. 이 연산자는 조건문의 참-거짓에 따라 표현식을 평가합니다. 파이썬 2.4부터 지원되었습니다.

아래에 기본 코드 청사진과,  조건 표현식을 사용한 예시들을 소개합니다.

##### 코드의 청사진

```py
condition_is_true if condition else condition_is_false
```

**예시:**

```py
is_fat = True
state = "fat" if is_fat else "not fat"
```

여러 줄로 된 if 문 대신 조건을 빠르게 테스트 할 수 있습니다. 종종 그것은 매우 도움이 될 수 있으며 코드를 작게 만들 수 있지만 유지 보수가 가능합니다.

좀더 모호하고 잘 사용되지 않는 튜플을 사용하는 다른 방법도 있습니다.  다음은 몇 가지 예제 코드입니다.

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

위 예제와 같은 방법은 많이 사용 되지 않고 파이썬 사용자들이 파이썬스럽지 않아 좋아하지 않습니다. 또한 튜플의 어디에 True, False 값을 넣어야 할지 혼동하기 쉽습니다.

또 if-else 삼항연산자에 반해, 튜플들의 요소 둘 모두 평가하기 때문에 튜플 삼항 연산자 사용을 피하는 것이 좋습니다.

**예시:**

```py
condition = True
print(2 if condition else 1/0)
#Output is 2

print((1/0, 2)[condition])
#ZeroDivisionError is raised
```

이것은 튜플 삼항 테크닉 때문인데, 튜플이 먼저 생성되고 그 다음 인덱스를 찾기 때문에 발생 합니다. if-else 삼항연산자는 일반적인 if-else 로직을 따라갑니다. 그러므로, 조건에 따라 에외를 발생시켜야한다거나 무거운 계산이 필요한 경우에는 사용하지 않는 것이 좋습니다.

