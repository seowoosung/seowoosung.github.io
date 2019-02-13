---
layout: post
title: python 기초 공부
date: 2018-10-21
categories: [Python]
---

## python 공부 시작할때 유용한 자료

[python 기초문법 유튜브 강좌](https://www.youtube.com/watch?v=JJmcL1N2KQs&index=6&t=0s&list=PLillGF-RfqbYeckUaD1z6nviTp31GLTH8)

[python 가상환경 구성 포스트](https://beomi.github.io/2016/12/28/HowToSetup-Virtualenv-VirtualenvWrapper/)

## 환경구성
IDE: Visual Studio Code 사용

### 1. mysql 설치하기
django에서 sqlite대신 mysql 사용할 예정. mysql 깔다가 OSError나면서 안되면 명령어 수행하기

 $ sudo apt-get install libmysqlclient-dev
 $ sudo pip install mysqlclient

### 2. xampp 설치하기
XAMPP: APM(Apache + PHP + MySQL) 환경을 한번에 설치할 수 있도록 하는 소프트웨어

[xampp 다운로드 링크](https://www.apachefriends.org/index.html)에서 환경에 맞는 설치파일 다운로드
설치파일 권한 설정하기
 $ chmod 755 [package name]

루트권한으로 실행하고 next누르면서 설치완료하면 된다.
 $ sudo ./[package name]

설치가 끝나면 xampp를 실행한다.
 $ sudo /opt/lampp/lampp start

xampp가 정상적으로 실행되면 http://localhost(apache가 실행중이면 화면보임), http://localhost/phpmyadmin(mysql 동작중이면 보임)에 들어가서 확인할 수 있다.

### 3. Database만들고 django 스키마 migrate하기
localhost/phpmyadmin 페이지로 이동해서 database(이름: djangoproject)를 create함
setting.py에서 DATABASES 설정을 바꿈
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'djangoproject',                  
        'USER': 'tibero',             
        'PASSWORD': 'tmax',                  
        'HOST': 'localhost',                     
        'PORT': '',                      
    }
}

참고로 database만 create했으면 user가 없기때문에 tibero user를 생성해줘야 정상동작한다.
settings.py를 수정했으면 shell에 아래와 같이 입력한다
$ python manage.py migrate

위의 명령어가 정상 수행되면 phpmyadmin페이지에서 table들이 생성된것을 볼 수 있다.
