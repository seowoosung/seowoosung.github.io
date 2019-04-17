---
layout: post
title: linux 주요 명령어들 
date: 2019-04-17
categories: [ETC]
---

* 리눅스 포트 개방
$ iptables -I INPUT 1 -p tcp --dport `포트번호` -j ACCEPT
* 리눅스 포트 닫기
$ iptables -D INPUT -p tcp --dport `포트번호` -j ACCEPT
* 리눅스 개방한 포트 확인
$ iptables -L | grep ACCEPT
