# 18. One-Liners

이 챕터에서는 정말 도움이 될 수 있는 한 줄짜리 파이썬 명령어\(one liner Python commander\)를 보여 드리겠습니다.

##### 간단한 웹서버\(Simple Web Server\)

네트워크를 통해 파일을 빨리 공유하고 싶은 적은 없었나요? 잘 오셨습니다. 파이썬은 딱 여러분을 위한 기능을 제공합니다. 네트워크로 서비스 하려는 디렉토리로 가서 터미널에 아래와 같은 명령어를 치세요.

```python
# python 2
python -m SimpleHTTPServer

# python 3
python3 -m http.server
```

##### 예쁘게 출력하기\(Pretty printing\)

list와  dictionary를 파이썬 repl안에 있는 예쁜 형태로 출력할 수 있습니다. 다음은 관련 코드 입니다.

```python
from pprint import pprint

my_dict = { 'name': 'Yasoob',
    'age': 'undefined', 
    '개성' : '멋짐' }
pprint(my_dict)
```

사전\(`dict)`에서는 좀 더 효율적입니다. 또, 간단하게 json파일을 예쁘게 출력하고 싶다면 아래와 같이 간단하게 할 수도 있습니다.

```python
cat file.json | python -m json.tools
```

##### 스크립트를 프로파일링하기\(Profiling a script\)

스크립트의 병목현상을 정확하게 짚고자 할떄  정말 유용합니다.

```python
python -m cProfile my_script.py
```

Note: `cProfile`이 C로 쓰여져서 `profile`보다 더 빠른 속도를 보여줍니다.

##### CSV to json

아래와 같이 터미널에서 실행시키면 됩니다.

```python
python -c "import csv, json; (print json.dumps(list(csv.reader(open('csv_file.csv')))))"
```

`csv_file.csv` 파일을 해당하는 파일로 바꾸는 것을 잊지마세요.

##### 리스트 중첩 줄이기 \(List Flattening\)

`itertools`패키지의 `itertools.chain.from_iterable`사용해서 간단하고 쉽게 리스트 중첩을 줄일 수 있습니다.  
간단한 예시를 보면,

```python
a_list = [[1, 2], [3, 4], [5, 6], [7, 8]]
print(list(itertools.chain.from_iterable(a_list)))
# Output: [1, 2, 3, 4, 5, 6]
```

##### 한-줄 명령어 생성자

클래스를 초기화할 때 많은 표준 할당을 피하려면,

```python
class A(object):
    def __init__(self, a, b, c, d, e, f):
        self.__dict__.update({k: v for k, v in locals().items() if k != 'self'})
```

좀 더 많은 한 줄 명령어들은 [파이썬 웹사이트](https://wiki.python.org/moin/Powerful%20Python%20One-Liners)에서 확인하실 수 있습니다.

