# 11. 가상 환경

`virtualenv`에 대해 들어본 적 있으신가요? 초보자인 경우에는 그것에 대해 들어 본 적이 없겠지만 숙련 된 프로그래머라면 툴 세트의 중요한 부분 일 수 있습니다.

그래서 `virtualenv`은 무엇일까요? `virtualenv`은 분리된 가상의 파이썬 환경을 만들어주는 툴입니다.  하나의 애플리케이션은 파이썬 버전 2의 라이브러리가, 다른 하나는  파이썬 버전 3의 라이브러리가 필요한 애플리케이션을 가지고 있다고 생각 해보세요. 어떻게 이 두개의 애플리케이션을 모두 사용하고 개발할 수 있을까요?

만약 이 모든 것들을 `/usr/lib/python2.7/site-packages`\(여러분의 플랫폼의 기본 저장소가 어딘가\)에 설치했다고 생각해보세요. 그러면 여러분이 업그레이드 해서는 안되는 패키지를 원치않게 업그레이드 할 가능성이 정말 높습니다. 

또 다른 경우에는 개발을 완료한 애플리케이션에서 사용중인 라이브러리를 더 이상 변경하고 싶지는 않지만 동시에 해당 라이브러리의 업데이트된 버전이 필요한 다른 애플리케이션을 개발하기 시작한다고 상상해 보세요.

그렇다면 어떻게 해야할까요? 이 시점이 `virtualenv`을 사용해야할 때입니다. 파이썬 응용 프로그램을 위한 격리된 환경을 만들고 전역으로 설치하는 대신 격리된 환경에 파이썬 라이브러리를 설치할 수 있습니다.

설치하려면 쉘에 다음 명령을 입력하십시오:

```
  $ pip install virtualenv
```

가장 중요한 명령어는: 

```
  $ virtualenv myvenv
  $ source myvenv/bin/activate
```

첫번째 명령은 `myproject`폴더 안에 분리된 가상환경을 생성하는 것이고, 두번째 명령어는 분리된 가상환경을 실행시킵니다. 

virtualenv를 생성하는 동안  virtualenv가 시스템의 `site-packages` 에 있는 패키지들을 사용하거나,  virtualenv의 `site-packages`에 있는 패키지들을 설치할지를 결정해야 합니다. 기본적으로 virtualenv는 전역 사이트 패키지에 대한 액세스 권한을 부여하지 않습니다.

  
virtualenv가 시스템 site-packages 에 액세스하게하려면 다음과 같이 virtualenv를 만들 때 --system-site-packages 스위치를 사용하십시오.

```
  $ virtualenv --system-site-packages mycoolvenv
```

자 이제 여러분은 그 어떤 라이브러리도 전역 라이브러리나 라이브러리나 다른 환경에 영향을 끼치지 않고 설치할 수 있습니다.  
다음을 입력하여 env를 끌 수 있습니다.

```
  $ deactivate
```

##### 추가 팁

bash 및 zsh 용 라이브러리 인 smartcd를 사용해서 cd로 bash \(또는 zsh\) 환경을 변경할 수 있습니다  
디렉토리를 변경할 때 `virtualenv`을 activate와 deactivate 해주기 때문에 정말 편합니다. 필자는 이것을 많이 사용하고 좋아합니다. [github](https://github.com/cxreg/smartcd)에서 더 많은 정보를 얻을 수 있습니다.

`virtualenv`에 대한 간단한 소개글이었고 이 외에도 정말 많이 있습니다. 더 알아보고 싶다면 이 [링크](http://docs.python-guide.org/en/latest/dev/virtualenvs.html)를 추천드립니다.

