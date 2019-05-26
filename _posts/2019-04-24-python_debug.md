---
layout: post
title: Django 디버깅
date: 2019-04-24
categories: [ETC]
---

# 디버깅 방법
## 1. 페이지 디버깅
브라우져에서 특정 url을 rendering하는 과정에서 http request나 수행한 sql문장 정보 등을 볼 수 있다.

대표적으로 django-debug-toolbar를 이용하는 방법이 있다. 

우선 패당 패키지를 설치한다.
$ pip install django-debug-toolbar
settings.py를 수정한다.
```python
INSTALLED_APPS = [
    ...
    'django.contrib.staticfiles',
    ...
    'debug_toolbar',
]

MIDDLEWARE = [
    ...
    'debug_toolbar.middleware.DebugToolbarMiddleware',
    ...
]

INTERNAL_IPS = (['*])

def custom_show_toolbar(request):
  return True
  
DEBUG_TOOLBAR_CONFIG = {
  'SHOW_TOOLBAR_CALLBACK': custom_show_toolbar,
  'INTERCEPT_REDIRECTS': False,
}
```
project의 urls.py에 아래 구문을 추가한다.
```python
from django.conf import settings
from django.conf.urls import include, url

if settings.DEBUG:
    import debug_toolbar
    urlpatterns = [
        url(r'^__debug__/', include(debug_toolbar.urls)),
    ]
```
## 2. 소스코드 디버깅
특정 url관련 정보가 아닌, 어떤 로직을 수행할때 source code를 따라가볼 수 있는 디버깅 방법이다.

### [1] gdb(cgdb)이용
cpython의 경우 c로 구현되어있기 때문에 gdb를 이용해서 디버깅 가능하다. 하지만 c언어 symbol을 갖고있어야 하기 때문에 
python 바이너리뿐 아니라 python의 소스코드도 필요하다. 

좀 복잡해보이긴 하지만 이미 실행중인 process에 붙어서 디버깅을 할 수 있다는 장점이 있다.
pyCharm의 경우 실행중인 process에 붙어서 디버깅하는 기능을 제공한다고 들었는데, VSCode는 해당 기능이 없는듯 하다.

### [2] pdb 이용
c나 c++에서 gdb를 이용하듯 매우 고전적인 방법으로 보인다. 단점은 소스코드에 직접 import pdb; pdb.set_trace()구문을 삽입해야 한다는 점이다. 

이 구문이 breakpoint가 된다. breakpoint를 추가했으면 server를 실행해본다.
```bash
 $python -m pdb manage.py runserver
 ```
디버거가 붙어있기 때문에 server가 바로 뜨지 않는다 
```pdb
 (Pdb)c
 ```
로 continue를 해줘야 server가 실행된다. server가 실행되고나서 해당 server에 request를 보내보면 pdb.set_trace()가 있는 line에 breakpoint가 잘 잡히는걸
확인할 수 있다.
 
### [3] ipdb이용
pdb와 매우 비슷한데 좀더 사용하기 편리하다. pdb가 들어가던 자리에 ipdb를 사용하면 된다. ipdb의 경우 default로 line이 3개가 뜨는데 바꾸고 싶다면
```bash
 $python -c 'import ipdb; print(ipdb)' 
 ```
위 명령어로 생성된 directory로 가서 ipdb 패키지의 __main__.py에서 set_trace 디폴트값을 바꿔주면된다.
```vim
 def set_trace(frame=None, context=3):   <--이 부분 찾아서 context를 늘려주면됨
 ```
### [4] django-pdb 패키지 이용
누군가가 [github](https://github.com/HassenPy/django-pdb)에 패키지를 구현해놓았다. pdb이용시 소스코드에 직접 pdb.set_trace()구문을 삽입해야하는 불편함을 해결한 방법이라고 적혀있긴한데, 따라해봤으나 잘 되지 않았다.

### [5] VsCode이용
처음에 vscode로는 process디버깅이 안되는줄 알았는데 Debug탭에서 Python:Django를 선택후 F5를 누르니 디버깅을 할 수 있었다. 

한번에 두 client 접속을 하면 안되는것 같고, page가 안뜰때 브라우저(혹은 브라우저탭)을 껐다가 키면 잘 동작한다. 

server의 ip, port를 바꿔주고 싶으면 launch.json의 Django항목을 찾아 arg에 해당 ip와 port를 추가해주면 된다.

virtualenv 내부의 app을 디버깅 하는경우 python을 명확하게 명시해주지 않으면 debug시에 default python이 불려서 제대로 수행되지 않는다.
이 경우 ModuleNotFoundError: No module named 'django' 등의 에러가 발생한다.

이를 해결하기 위해 launch.json파일에 "pythonPath" : "${env:HOME}/.virtualenvs/py36/bin/python"을 추가해줘야한다. 참고로 py36은 내 virtualenv의 경로다.
