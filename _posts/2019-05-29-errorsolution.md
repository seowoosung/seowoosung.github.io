---
layout: post
title: linux 문제상황 해결책 모음
date: 2019-05-29
categories: [ETC]
---

### ubuntu 16.04에서 wifi 안되던 문제
* [문제1]$ sudo lshw -class network 했을때 무선랜카드(Broadcom사용중)가 잡히긴 하는데 unclaimed 표시 되어있었음
* [문제1-2]$ iwconfig 시에 wireless 관련 정보가 아예 뜨지않음
* [해결] 유선으로 인터넷 연결 후 $ sudo apt-get install bcmwl-kernel-source

* [문제2] 랜카드 동작은 확인됐지만 인터넷 연결이 안되는 경우
* [해결] broadcom의 경우 별도의 driver설치가 필요한것으로 보임. $ sudo apt-get install firmware-b43-installer

### 기계식 키보드 한글 셋팅
* ibus 한글 다운로드는 첫번째 링크를, 기계식 키보드의 Alt_R을 Hangul로 설정하는 부분은 두번째 링크를 참조
* https://dgkim5360.tistory.com/entry/how-to-install-ibus-hangul-for-ubuntu-desktop
* https://hanmaruj.tistory.com/6

### ubuntu18.04 LTS 부팅시에 멈춤 현상
* $ sudo vim /etc/default/grub에서 quiet splash -> nomodeset quiet splash 로 바꾸니 일단은 잘 돌아감
