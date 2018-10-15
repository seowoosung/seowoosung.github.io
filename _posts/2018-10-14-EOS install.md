---
layout: post
title: EOS 설치하기
date: 2018-10-14
categories: [Block Chain/EOS]
---
## 설치 전 준비사항
***
EOSIO DApp 개발을 위해 우선 EOSIO의 node를 설치해봤다.  
오픈소스이기 때문에 직접 source를 다운받아서 빌드할 수 있지만, [EOSIO portal](https://developers.eos.io/eosio-home/docs)에서는 docker를 이용한 설치 가이드가 있어서 docker를 이용해 설치했다.  
[EOSIO portal](https://developers.eos.io/eosio-home/docs)에도 아주 자세히 설명이 써있지만 한번더 나름대로 정리해보았다. 

현재 EOSIO는 window를 지원하지 않기 때문에 window 머신에서는 VM을 이용해 linux를 설치해야한다.  
나도 Virtual Box에 ubuntu 18.04를 설치했다. -> [VM에 ubuntu설치하기](http://programmerchoo.tistory.com/37)  
또 docker를 이용하기 때문에 [docker 설치](https://docs.docker.com/install/linux/docker-ce/ubuntu/#set-up-the-repository)도 필요하다.  
다들 링크에 있는대로만 따라하면 어렵지 않게 설치할 수 있다.

## docker로 EOSIO container 설치하기
***

$ docker pull eosio/eos 실패하면 sudo붙이기

cleos --wallet-url http://127.0.0.1:5555 wallet list keys에서 
Error 3120006: No available wallet뜸.

rebooting하래서 rebooting 하니까 

Error response from daemon: Container *** is not running 이 뜸
시킨대로 sudo docker start eosio하니까 다시 잘됨

혹시 start해도 안되면,
우선 sudo docker rm eosio로 eosio컨테이너 지운다음에 
docker run부터 다시수행함. 그러니까 이전 상황까지는 왔는데

cleos --wallet 이 명령어 대신 cleos wallet create한번 해봄 그랬더니 
unable to connect to keosd~ 뜸

우선 keosd 프로세스를 킬하고 cleos wallet list하면 원하는대로 Wallets:[]가 뜨긴하는데
맞는지는 모르겠음 우선 넘어가야지.
