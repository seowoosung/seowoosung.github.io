---
layout: post
title: Django 프레임워크 설치 및 실행
categories: [Architecture]
---

django는 python으로 구성된 웹 애플리케이션 프레임워크로 front/backend 구현이 가능하며, 
테스트용 웹서버까지 띄울 수 있다. 이번 포스트에서는 django의 내장 웹서버를 실행해서 첫화면을 띄워볼 것이다.

우선 django-sample이라는 이름의 project 생성한다.
{% highlight bash %}
$ pip install django
$ django-admin startproject django_sample
{% endhighlight %}

django_sample이라는 이름의 directory가 생기고 해당 directory에 들어가면 manage.py와 django_sample directory가 보인다.

django_sample이 두개 있으면 헷갈리므로 이름을 app으로 바꿔준다.(관련된 이름을 grep으로 검색해서 다 바꿔줘야함)
바꾸고 난 뒤 모습은 아래와 같다
{% highlight language %}
django_sample => [app, manage.py]
{% endhighlight %}

이제 migrate를 하고 server를 띄운다. migrate는 django의 코드를 실제 DB에 반영하는 명령어이다. 해당 명령어를 수행하고 나면 Django를 실행하는데 필요한 기본 테이블들이 생성된다. 별도의 Database를 설정하지 않으면 python에서 기본 제공되는 sqlite DB(db.sqlite3)가 생성된다.

{% highlight bash %}
$ python manage.py migrate
$ python manage.py runserver
{% endhighlight %}

정상적으로 django server가 실행되면 localhost:8000에서 아래와 같은 화면을 볼 수 있다.
![_config.yml]({{ site.baseurl }}/assets/images/Screenshot from 2020-05-09 23-18-42.png)

django server process가 WSGI interface를 통해 django application을 호출하게 된다.
django application은 필요하다면 sqlite로부터 DB의 정보를 불러온다.

> 주의: django runserver는 내장 python process로 실행되며, python은 GIL로 구현되어 있기때문에 멀티쓰레딩을 지원하지 않는다. 따라서 runserver를 사용하게 되면 단일 쓰레드로 모든 요청을 처리하게 되므로 production 환경에서는 사용하지 않아야한다.

![_config.yml]({{ site.baseurl }}/assets/images/django_base.png)
