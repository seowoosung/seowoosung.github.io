---
layout: post
title: 분산 트랜잭션(Distributed Transaction) 관련 용어 정리
date: 2018-10-09
categories: [ETC]
tags: 
  - database
---

출처 : [x/open문서](http://pubs.opengroup.org/onlinepubs/009680699/toc.pdf), [tmax technet](https://technet.tmaxsoft.com/upload/download/online/tibero/pver-20150504-000001/tibero_dev/ch04.html)
# 관련 용어
***
## Distributed Transactions
global transaction이라고도 불리며 여러개의 분산된 resource들(예: Database) 각각에 대한 transaction들을 하나의 transaction으로 묶은 것을 의미한다. 이 경우 하나의 resource라도 실패하면 전체를 rollback하게 된다.

## Distributed Transaction Processing(DTP)
DTP 아키텍처는 여러 Application이 Transaction Manager를 이용하여 여러 다른 Resource Manager가 제공하는 리소스를 공유 할 수있게 해주는 표준 아키텍처 또는 인터페이스를 의미한다.

![_config.yml]({{ site.baseurl }}/assets/images/dtp.png)

## Application Program(AP)
DTP에서 Application program은 transaction boundaries를 정의하고 특정 transaction들을 수행한다. TM에게 transaction의 시작과 끝, commit등을 요청하고 RM에게는 작업(ex: RM이 DBMS인 경우 SQL문 Execute)을 요청한다.

## Resource Manager(RM)
RM은 컴퓨터의 공유 리소스를 관리한다. 공유 리소스의 예시로는 프린터기, Database등이 있으며 RM의 예시로는 프린터 드라이버나 Oracle 및 Tibero같은 DBMS가 있다.

## Transaction Manager(TM)
각 RM별로 XID를 생성하여 transaction 진행을 관리하고 전체 RM들의 transaction을 commit하거나 rollback하는 기능을 수행한다. 

## Transaction Processing Monitor(TPM)
통신 프로그램 및 프로세스 관리, 트랜잭션 관리 등 관리의 어려운 부분을 쉽게 처리해주는 미들웨어의 한 종류로 오라클의 Tuxedo나 티맥스의 Tmax와 같은 제품이 있다. TM은 XA interface를 통해 RM과 통신하게 되는데 TPM을 사용할 경우 이러한 부분은 직접 코딩하지 않아도 된다는 장점이 있다. TPM을 사용하면 복잡하게 트랜잭션고민할 필요 없이 DTP를 쉽게 구현할 수 있다.

## Transaction Branches
하나의 Resource Manager에 포함된 작업단위를 의미한다. Resource가 DB인 경우 각 branch는 DBMS 내부의 로컬 트랜잭션을 의미한다. RM 하나에 여러 Transaction Branch가 존재할 수 있다.

## Two-Phase Commit Protocol
DTP 환경에서 트랜잭션에 참여하는 모든 데이터베이스(resource)가 정상적으로 수정되었음을 보장하는 두 단계 커밋 프로토콜이다.

First Phase는 각 TM이 데이터베이스 노드에 커밋을 하기 위한 prepare 메시지를 보낸다.

Second Phase(또는 Commit Phase)에서는 TM이 참여한 모든 데이터베이스 노드로부터 prepare 완료 메시지를 받을 때까지 대기한다.  
prepare메시지 중 하나라도 ok가 아니라면 전체 데이터베이스에 롤백 메시지를 보낸다. 만약 모든 데이터베이스 노드로부터 prepare ok 메시지를 받으면 전체 데이터베이스에 커밋 메시지를 보낸다

![_config.yml]({{ site.baseurl }}/assets/images/2phase.png)

## XA
XA는 분산트랜잭션 환경에서 TM과 RM사이에 통신을 담당하는 하나의 표준화된 interface를 의미한다. Oracle, Tibero, DB2등 벤더사별로 해당 interface에 맞는 동작을 구현해서 제공하고 있다. 즉 xa_open의 경우 오라클은 내부적으로 oracle server에 connection을 맺는 코드를 구현하고, 티베로는 내부적으로 tibero server에 connection을 맺는 코드를 구현한다.

![_config.yml]({{ site.baseurl }}/assets/images/xa.png)

XA의 c언어 interface는 아래와 같다.

| 함수 | 설명 |
|:--------|:--------|
| xa_open | 리소스 매니저(Resource Manager)에 접속한다. |
| xa_close | 리소스 매니저에서 데이터베이스 접속을 해제한다. |
| xa_start | XID값을 주고 새로운 트랜잭션을 시작하거나, 이미 존재하는 트랜잭션에 현재 프로세스를 연결한다. |
| xa_end | XID의 TX에서 현재 프로세스를 분리한다 |
| xa_rollback	 | XID의 TX를 롤백한다. |
| xa_prepare | XID의 TX에 대한 커밋을 준비한다. Two-phase commit의 First Phase이다. |
| xa_commit | XID의 TX에 대한 커밋을 완료한다. Two-phase commit의 Second Phase이다. |
| xa_recover | prepare 상태인 트랜잭션의 목록을 검사하여 커밋이나 롤백을 수행한다. |
| xa_forget | XID의 TX가 이미 처리된 경우 로그 기록을 삭제한다. |
