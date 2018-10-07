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

유니코드란
=============
- 유니코드: 전 세계의 모든 문자를 다루도록 설계된 표준 문자 전산 처리 방식(키와 값이 1:1로 매핑된 형태의 코드)
- EUC-KR과 차이점 : 예를 들어 EUC-KR로 "안녕"을 나타내면 "BE C8(안) B3 E7(녕)"인데 만약 이렇게 인코딩된 파일을 사용자가 실수로 EUC-JP로 디코딩한다면 BE C8 B3 E7에 해당하는 일본어(照括)로 출력이 된다. 하지만 유니코드는 전세계의 모든 문자가 unique한 값을 가지기 때문에 UTF8-KR, UTF8-JP같은 개념이 없고 모든 나라의 문자가 UTF-8로 인코딩/디코딩 될 수 있다. 
- 유니코드는 하나의 코드셋으로 모든 문자를 나타내야하고, 가변길이라는 특성때문에 특별한 처리를 거쳐 인코딩되게 된다.
- 예를 들어 "안녕" 이라는 한글 문자열이 있을때 유니코드표를 보면 '안'=C548, ‘녕'=B155이다.
- “C5 48(안) B1 55(녕)”를 UTF-8로 인코딩하게 되면 000800-00FFFF영역이므로 3+3 = 6byte가 되며 해당 값은 “EC 95 88(안) EB 88 95(녕)” 이다
“C5 48(안) B1 55(녕)”를 UTF-16으로 인코딩하게 되면 000800-00FFFF영역이므로 2+2 = 4byte가 되며 해당 값은 “C5 48(안) B1 55(녕)" 이다

![_config.yml]({{ site.baseurl }}/images/unicode.png)

character encoding 과정
=============
- keyboard에서 (한/영)키를 눌러서 한글입력으로 전환한 후에 "안녕"을 입력한 상황을 생각해보자.
- 'ㅇ', 'ㅏ', 'ㄴ', 'ㄴ', 'ㅕ', 'ㅇ'   여섯개의 버튼이 눌리면 메인보드, CPU를 거쳐 kernel에 인터럽트가 보내지게 된다. 
(1) 커널은 해당 인터럽트를 키보드상에서의 물리적 위치값(key code)으로 변환해서 X window system으로 보낸다. 
(2) X window system에서는 함께 눌린 modifier key(Ctrl, Shift 등)을 분석해서 key code를 key symbol로 변환한다. key symbol은 'ㅅ','ㅆ',  'A’, ‘Left_alt’와 같은 character형태로 표현되는 symbol을 의미한다. 해당 key symbol은 GUI application이나 terminator로 전달되게 된다. 
(3) terminal emulator에서는 해당 key symbol을 받아 설정된 charset encoding에 맞게 byte값으로 변환하게 된다. 예를 들어 terminal인코딩이 UTF-8이라면 terminator는 “EC 95 88(안) EB 88 95(녕)” 의 6byte로 변환 후  vim이나 tbsql처럼 terminator위에서 동작하는 application으로 전달된다. gedit과같이 terminal과 별도로 동작하는 application은 (3)의 과정이 application내부에서 일어나게 된다.
(4) application에 encoding된 byte값이 저장된다.
