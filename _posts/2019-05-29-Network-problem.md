---
layout: post
title: linux에서 network 문제 있을때
date: 2019-05-29
categories: [ETC]
---

### ubuntu 16.04에서 wifi 안되던 문제
* [문제1]$ sudo lshw -class network 했을때 무선랜카드(Broadcom사용중)가 잡히긴 하는데 unclaimed 표시 되어있었음
* [문제1-2]$ iwconfig 시에 wireless 관련 정보가 아예 뜨지않음
* [해결] 유선으로 인터넷 연결 후 $ sudo apt-get install bcmwl-kernel-source

* [문제2] 랜카드 동작은 확인됐지만 인터넷 연결이 안되는 경우
* [해결] broadcom의 경우 별도의 driver설치가 필요한것으로 보임. $ sudo apt-get install firmware-b43-installer
