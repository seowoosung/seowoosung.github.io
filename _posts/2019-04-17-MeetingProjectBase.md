---
layout: post
title: meeting앱 개발 프로젝트 프레임 구성하기
date: 2019-04-17
categories: [MeetingApp]
---

## 파일(static, template) 관리하기
django 프로젝트 전역적으로 사용할 file들은 staticfiles기능을 이용해 관리한다.
우선 static file 디렉토리로 사용할 content/assets를 생성한다. 
그다음 settings.py에 아래의 구문을 추가한다.

```vim
STATICFILES_DIRS = [
  os.path.join(CONTENT_DIR, 'assets'),
]
```

template파일에서 assets/css/style.css를 사용하는 예시는 아래와 같다. 

```python
  {% load staticfiles %}
  <link href="{% static 'css/style.css' %}" rel="stylesheet">
```

만약 template을 전역적으로 사용하고 싶으면 content/templates를 만들고 
settings.py의 TEMPLATES의 'DIRS'에 os.path.join(CONTENT_DIR, 'templates'),
를 추가한다. 사용할때는 templates디렉토리로부터의 상대위치를 사용하면 된다. 본 프로젝트에서는 page.html을 상속받아서 나머지 페이지들을 구성할 예정이다. 
page.html은 content/templates/layouts/default/page.html에 위치해있다.

```html
{% extends 'layouts/default/page.html' %}
```

## 다국어지원
우선 settings.py에 다국어 지원 관련 설정을 해준다

```vim
from django.utils.translation import ugettext_lazy as _
#미들웨어 추가
MIDDLEWARE = [
    ...
    'django.middleware.locale.LocaleMiddleware',
]

LANGUAGE_CODE = 'ko-KR'
#지원할 다국어 언어들 설정
LANGUAGES = [
    ('ko', _('Korean')),
    ('en', _('English')),
]
#content/locale을 다국어지원 관련 file이 생성될 위치로 지정
LOCALE_PATHS = (
    os.path.join(CONTENT_DIR, 'locale'),
)
```

다국어 지원은 템플릿 파일이나 python 파일 모두에서 사용될 수 있다.
python 파일에서 사용될경우 ('word', _('Word')) 처럼 사용한다.
템플릿 파일의 경우 {{ _('Word') }} 로 사용하면 된다. 아래처럼 i18n태그를 이용할 수 도 있다.

```html
{% load i18n %}
{% trans 'Word' %}
```

이제 실제로 변환 파일을 수정할 차례다. 위의 예시에서처럼 다국어 지원 구문을 사용한 상태에서 아래의 명령어를 쉘에서 수행한다.

```bash
python manage.py makemessage -a  # 잘 수행되지 않는경우 아래의 명령어로 직접 locale 파일들을 생성해준다
python manage.py makemessages -l ko
python manage.py makemessages -l en
```

이제 content/locale파일에 가보면 ko폴더에 django.po가 생성되어있다. 해당 파일에 들어가서 번역될 message를 입력해주면 된다.
번역 파일 수정을 완료했으면 컴파일한다.

```bash
python manage.py compilemessages
```

## bootstrap 사용하기
bootstrap을 사용하기 위해서는 
settings.py의 INSTALLED_APPS에 'bootstrap4'를 추가한다. 실제 template에서는 아래처럼 사용한다.

```html
<head>
<link href="{% static 'vendor/bootstrap/css/bootstrap.min.css' %}" rel="stylesheet">
</head>

<script src="{% static 'vendor/jquery/jquery-3.3.1.min.js' %}"></script>
<script src="{% static 'vendor/bootstrap/js/bootstrap.min.js' %}"></script>
```

bootstrap에서 필요한 자바스크립트를 추가하고, bootstrap.min.js를 사용하기위해 jquery 스크립트도 추가한다.
스크립트문은 읽는데 오래걸리기 때문에 맨 밑에 추가한다.

