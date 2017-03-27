# 17. Lambdas

람다는 1줄 함수 입니다. 다른 프로그래밍 언어에서는 익명 함수 라고도 부릅니다. 프로그램 내에서 함수를 재사용 하지 않으려는 경우 lambda 함수를 쓸 수 있습니다. 일반함수와 형태도 비슷하고 비슷하게 작동합니다. 

##### 코드의 청사진:

```python
lambda argument: manipulate(argument)
```

##### 예시

```python
add = lambda x, y : x + y

print(add(3, 5))
# Output: 8
```

아래에 유용한 예시 들과, 널리 사용하는 몇몇 방법들을 소개합니다.

##### 리스트 정렬하기

```python
a = [(1, 2), (4, 1), (9, 10),(13, -3)]
a.sort(key=lambda x: x[1])

print(a)
# Output: [(13, -3), (4, 1), (1, 2), (9, 10)]
```

##### 병렬로 리스트 정렬하기

```python
data = zip(list1, list2)
data.sort()
list1, list2 = map(lambda t: list(t), zip(*data))
```



