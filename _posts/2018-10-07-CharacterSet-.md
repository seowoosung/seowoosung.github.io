---
layout: post
title: Character set과 Character encoding 정리
---

기본개념
=============
- character encoding: 문자나 기호들의 집합을 컴퓨터에서 표현하는 방법.(UTF8, UTF16)
- character set: 정보를 표현하기 위한 글자들의 집합을 정의한 것. 한 문자 집합을 여러 문자 인코딩에서도 사용가능(ex: 유니코드)

다양한 한글 character encoding 종류
=============
1. EUC-KR
- ASCII 영역(127이하)은 그대로 1byte의 동일값을 사용.
- 한글은 주로 2바이트로 표현.
- 한글 완성형 인코딩
2. MSWIN949(CP949)
- MS사가 도입한 코드페이지.
- 한글은 주로 2바이트로 표현.
- EUC-KR의 확장이며, EUC-KR과 하위 호환성이 있다.
3. UTF-8
- 가변길이 유니코드 인코딩(1byte ~ 4byte)
- ASCII 영역(127이하)은 그대로 1byte의 동일값을 사용.
- 인터넷 사이트에서 가장 많이 쓰이는 인코딩.
- 엔디안에 상관없이 똑같이 읽을 수 있음
- 한글은 주로 3바이트로 표현
- 리눅스나 macOS에서 운영체제 자체의 인코딩 형식을 UTF-8로 통일.(window는 레거시 호환을 위해 UCS-2와 병행)
4. UCS-2 (Universal Character Set)  
- 유니코드 이전에 사용된 국제 인코딩 규격
- 2byte로 이루어져 있으며(65,536개의 영역을 사용), 이 영역을 BMP(기본 다국어 평면)이라고 한다.
- ascii영역의 문자들도 각각 2byte로 표현되므로 변환없이는 영문자도 호환되지 않는다.
5. UTF-16
 - 가변길이 유니코드 인코딩(2byte, 4byte)
 - 2byte영역(BMP영역)까지는 UCS-2와 동일하나, 가변길이 부호화를 통해 4byte영역까지 확장함으로써 더 많은 문자를 표현할 수 있다.
- ascii영역의 문자들도 각각 2byte로 표현되므로 변환없이는 영문자도 호환되지 않는다.



![_config.yml]({{ site.baseurl }}/images/unicode.png)

