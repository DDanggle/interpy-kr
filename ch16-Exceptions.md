# 16. Exceptions

예외 처리는 일단 당신이 마스터하면 엄청난 힘을 부여하는 기술입니다. 예외처리를 다루는 방법을 몇가지 보여드리겠습니다.

기본적으로 try/except 문을 사용한다는 것을 잘 알고 있습니다. 예외가 발생할 수 있는 코드를 `try`  블록에 넣고, 예외 처리는 `except`  블록에 구현 됩니다. 간단한 예시부터 보겠습니다.

```python
try:
    file = open('test.txt', 'rb')
except IOError as e:
    print('An IOError occured. {}'.format(e.args[-1]))
```

위 예제 에서는 오직 IOError 예외만 처리했습니다. 대부분의 입문자들은 다중 예외처리가 가능하다는 사실을 잘 모릅니다.

### 16.1 여러가지 예외처리를 함께 하는 법:

세 가지 방법을 통해 다중 예외처리를 해 볼 것입니다. 첫 번째는 발생할 수 있는 모든 예외 들을 튜플에 넣는 것입니다.  예를 들면,

```python
try:
    file = open('test.text', 'rb')
except (IOError, EOFError) as e:
    print("An error occured. {}".format(e.args[-1]))
```

또 다른 방법은 각각 except 블록을 만들어서 예외처리를 하는 방법입니다. 원하는 만큼 except 블록들을 만들 수도 있습니다.

```python
try:
    file = open('test.txt', 'rb')
except EOFError as e:
    print("An EOF Error occured")
    raise e
except IOError as e:
    print("An error occured")
    raise e
```

첫 번째 예외 처리 블록에서 예외처리가 안 되었을 경우에는 다음 블록에서 처리됩니다. 마지막 방법은 모든 예외처리를 한 번에 처리하는 것입니다.

```python
try:
    file = open('test.txt', 'rb')
except:
    # some login if you want
    raise
```

이 방법은 프로그램 내에서 발생할 수 있는 예외에 대해 잘 모를 때 사용하면 도움이 될 것입니다.

### 16.2 `Finally`문

try 문 안에 메인 코드를 감싸서 넣습니다. 감싼 후에 except문 안에 try문 안에서 예외를 만났을 때 실행되는 코드들을 넣습니다. 다음 예시에서는`finally` 라는  세 번째 절을 만들어봅시다. `finally` 문 안에 감싸진 코드는 예외 발생 여부와 상관 없이 실행됩니다. 스크립트 수행 후에 정리 작업에 사용될 수 있습니다. 여기 간단한 예시가 있습니다.

```python
try:
    file = open('test.txt', 'rb')
except IOError as e:
    print('An IOError occurred. {}'.format(e.args[-1]))
finally:
    print("예외 발생 여부와 상관 없이 출력 됩니다!!!")

# Output: An IOError occurred. No such file or directory
# 예외처리가 없어도 출력이 됩니다!!!
```

### 16.3 `try/else` 문

종종 예외가 발생하지 않았을 때 실행하기 원하는 코드가 있습니다. 이런 경우 `else`문을 써서 쉽게 할 수 있습니다. 에러가 발생되지 않을 때 실행되는 경우라면 그냥 `try` 문 안에 넣어버리면 안되냐고 질문할 수도 있을 것입니다. 그렇게 하면 코드의 모든 예외들이 `try`에서 잡히게 되므로 원하지 않는 방법입니다. 많은 사람들은 많이 사용하지않고, 저 조차도 많이 사용하지 않는 방법입니다. 다음 예제를 보겠습니다.

```python
try:
    print("예외가 없을 것이라고 확신합니다")
except:
    print("예외")
else:
    # try에서 예외가 발생하지 않는 경우에만 실행해야하는 코드.
    # 예외는 잡히지 않아야합니다.
    print("예외가 발생하지 않을 떄만 실행됩니다. 그리고 여기의 오류는 잡히지 않을 것입니다.")
finally:
    print("모든 케이스에서 실행됩니다.")

# Output: 예외가 없을 것이라고 확신합니다
# 예외가 발생하지 않을 떄 만 실행됩니다.
# 모든 케이스에서 실행됩니다.
```

이 `else`문은 예외가 없을 경우에만 실행되고, `finally`문이 실행되기 이전에 실행됩니다.

