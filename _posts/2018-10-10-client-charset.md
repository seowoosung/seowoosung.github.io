---
layout: post
title: 클라이언트와 character encoding
date: 2018-10-10
categories: [ETC]
---

### 1. client charset
- tibero client의 charset을 나타내며 default는 MSWIN949다. client charset을 설정하는 방법은 두가지가 있는데 첫번째는 OS 환경변수 셋팅하는 방법이 있다. 

![_config.yml]({{ site.baseurl }}/images/export_nls.png)

- 두번째로는 tbdsn.tbr에 셋팅해주는 방법이 있다. tbdsn.tbr의 설정이 OS 환경변수 설정을 오버라이딩한다.  
client driver는 TB_NLS_LANG으로 셋팅된(없으면 default값) charset으로 character를 인식하며, server에 메시지를 보내거나 받을때 해당 charset을 기준으로 server charset과 변환이 일어난다. 참고로 TB_NLS_LANG이 유효하지 않은 값이면 tbsql 로그인할때 error가 발생한다.  
아래 그림은 $TB_HOME/client/config/tbdsn.tbr파일이다.

![_config.yml]({{ site.baseurl }}/images/tbdsn.png)

### 2. server charset
- tibero server에 저장되는 data의 charset을 의미한다. 처음에 db를 구성할 때 설정가능하다. 경우에 따라 db를 다시 마운트하는 등 복잡한 과정을 거쳐서 charset을 변경할 수 있다.   
> [참고] 티베로에서 다루는 charset이름은 EUCKR, UTF8, ASCII, MSWIN949, UTF16, SJIS, JA16SJIS, JA16SJISTILDE, JA16EUC, JA16EUCTILDE, VN8VN3, GBK, RU8pC866 등이 있다. 현재 티베로의 default server charset은 mswin949다. 오라클의 경우 UTF8 대신 AL32UTF8등을 사용하므로 관련내용은 오라클 문서를 참고 바람.

### 3.  server ncharset
- tibero server의 NCHAR, NVARCHAR컬럼에 저장되는 data의 charset을 의미한다. server의 charset과 다를 수 있으며, UTF8, UTF16 두가지 인코딩이 가능하다. 만약 N이 SQL 문자열 앞에 붙으면 그 뒤에 작은 따옴표로 감싼 부분은 변환하지 않고 그대로 저장한다. 
> ex: insert into tbl values(N’안녕');

### 4. character encoding 테스트 
- “안녕"이라는 문자열을 insert할 한다. 참고로 안녕은 UTF8로 “EC 95 88(안) EB 88 95(녕)”, EUCKR로는 "BE C8(안) B3 E7(녕)".
- insert한 문자열이 server에 어떻게 저장되는지 보기위해 dump값을 select한다.

**[상황1]** terminal encoding이 EUCKR, client charset($TB_NLS_LANG)이 EUCKR, server charset이 UTF8일때  
![_config.yml]({{ site.baseurl }}/images/insert_utf8.png)

server charset에 맞게 UTF8로 잘 변환돼서 들어간다.

**[상황2]** terminal encoding이 UTF8, client charset($TB_NLS_LANG)이 EUCKR, server charset이 UTF8일때  
![_config.yml]({{ site.baseurl }}/images/insert_error.png)

client charset을 EUCKR로 인식하고 server charset에 맞게 변환하려 했으나 실제 input은 UTF8이기 때문에 변환이 실패하고 63(?의 아스키값)이 들어간다.

**[상황3]** terminal encoding이 EUCKR, client charset($TB_NLS_LANG)이 UTF8, server charset이 UTF8일때  
![_config.yml]({{ site.baseurl }}/images/insert_euckr.png)

client charset과 server charset이 동일하기 때문에 input data가 변환없이 EUCKR인코딩 그대로 들어간다. 하지만 나중에 db에 저장된 문자열을 출력하고자 할때 server charset이랑 실제 저장된 encoding이랑 맞지 않기때문에 깨져버린다.  

### 5. character set 문제발생시 확인 방법(tibero기준)
#### 1. character set관련 변수들 확인하는 방법
- server charset, server ncharset:   SQL>select * from sys._vt_nls_character_set;
- TB_NLS_LANG(client charset): $echo $TB_NLS_LANG
- 터미널 인코딩: terminal emulator마다 인코딩 셋팅정보가 있음.
- source file의 인코딩: $file -Bi file_name
- NLS_LANGUAGE(Date타입 출력과 관련, 이 외에는 상관없다): SQL>select * from nls_session_parameters;

### 2. 문제상황과 해결방법
tbsql이나 클라이언트에서 값을 insert후에 select한 상황에서 발생하는 문제는 크게 두가지 케이스로 나뉜다. 참고로 charset conversion시에 유효하지 않은 범위의 byte값이 들어오면 ?로 변환된다. app내부에서는 charset conversion이 발생하지 않는 경우를 가정한다.  
#### [상황 1] ???로 출력되는 경우  
데이터를 테이블에 insert하는 과정에서 문제가 생겼을 가능성이 매우 높다. 즉 client에서 server로 charset 변환을 하면서 문제가 생겼을 가능성이 높다.따라서 tbsql을 통해 insert한 경우 terminal의 인코딩과 TB_NLS_LANG값이 동일한지 확인해야하고, 기타 client 드라이버를 이용해 구현한 app에서는 c/c++등의 source파일의 인코딩과 TB_NLS_LANG값이 동일한지 확인해야한다.  
#### [상황 2] 글자가 깨져서 출력되는 경우  
데이터를 테이블에서 select해오는 과정에서 문제가 생겼을 가능성이 매우 높다. client에서는 server의 data를 client환경의 charset으로 변환한다. 이 과정에서 변환은 제대로 됐지만 변환된 charset이 클라이언트의 환경과 맞지 않을 가능성이 매우 높다.  
따라서 tbsql의 경우 terminal의 인코딩과 TB_NLS_LANG값이 동일한지 확인해야 하고, client driver(JDBC등)를 이용해 구현한 app에서는 app자체의 인코딩 설정까지 확인해야 한다.  

### 6. 결론
- 글자가 깨진다면 터미널의 인코딩, source파일의 인코딩, TB_NLS_LANG, app의 인코딩이 동일한지 확인해봐야 한다.
