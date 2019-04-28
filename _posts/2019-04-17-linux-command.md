---
layout: post
title: linux 주요 명령어들 
date: 2019-04-17
categories: [ETC]
---

### 리눅스 포트관련
* 리눅스 포트 개방
```bash
$ iptables -I INPUT 1 -p tcp --dport `포트번호` -j ACCEPT
```
* 리눅스 포트 닫기
```bash
$ iptables -D INPUT -p tcp --dport `포트번호` -j ACCEPT
```
* 리눅스 개방한 포트 확인
```bash
$ iptables -L | grep ACCEPT
```

### gdb관련
* process에 gdb 붙으려고 할때 ptrace permission에러가 나면 아래 명령어 수행
```bash
$ sudo bash
# echo 0 > /proc/sys/kernel/yama/ptrace_scope
```

### booting 관련
* ubuntu 18 이상에서 booting중에 'started user manager for uid ~' 뜨면서 hang 걸릴때
```bash
$ vim /etc/default/grub
```
```vim
GRUB_CMDLINE_LINUX_DEFAULT="nomodeset"
```
이렇게 바꾸면 우선 booting message도 볼 수 있다. 그다음 display manager를 lightdm으로 바꿔준다
```bash
$ sudo apt-get install lightdm
$ sudo dpkg-reconfigure lightdm
$ sudo reboot
```
