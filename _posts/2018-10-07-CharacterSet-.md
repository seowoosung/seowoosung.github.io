---
layout: post
title: Character set과 Character encoding 정리
date: 2018-10-07
categories: [character set]
---

기본개념
=============
***
- character encoding: 문자나 기호들의 집합을 컴퓨터에서 표현하는 방법.(UTF8, UTF16)
- character set: 정보를 표현하기 위한 글자들의 집합을 정의한 것. 한 문자 집합을 여러 문자 인코딩에서도 사용가능(ex: 유니코드)

둘의 차이점을 유니코드의 예시를 통해 알아보자.  
유니코드라는 character set은 각 글자가 특정 bytes에 대응되는 집합을 의미한다.  
예를 들어 **'안'**은 유니코드로 **"C5 48"**이라는 값과 대응되고, **'녕'**은 **"B1 55"**라는 값에 대응된다.   

UTF8과 UTF16은 모두 유니코드라는 character set을 이용한 인코딩 방식을 의미한다.  
인코딩은 쉽게 말해 컴퓨터가 이해할 수 있는 byte값으로 변환함을 의미한다. 따라서 **'안'**을 UTF8로 인코딩 한다는 의미는 **"C5 48"**이라는 값을 규칙에 따라 특정 byte값으로 변환하겠다는 의미이고, 이는 **“EC 95 88(안) EB 88 95(녕)”**로 변환된다. 비슷하게 **'안'**을 UTF16으로 인코딩하면 **"C5 48"**이라는 값이 규칙에 따라 **“C5 48(안) B1 55(녕)"**로 변환되게 된다.

다양한 한글 character encoding 종류
=============
***
### 1. EUC-KR  
 ASCII 영역(127이하)은 그대로 1byte의 동일값을 사용한다. 한글은 주로 2byte로 표현되며 한글 완성형 인코딩이다. 한글 완성형은 '가'를 'ㄱ'와 'ㅏ'로 나누지 않고 '가' 자체가 하나의 byte값에 대응됨을 의미한다. 반대 표현으로는 한글 조합형이 있으며, 현재는 거의 사용되지 않는다.

### 2. MSWIN949(CP949)  
 MS사가 도입한 인코딩 방식으로 한글은 주로 2byte로 표현한다. EUC-KR을 확장했기 때문에 하위 호환성이 있다. EUC-KR이 표현할 수 없는 한글 문자도 표현 가능하다.

### 3. UTF-8  
 가변길이 유니코드 인코딩(1byte ~ 4byte)으로 ASCII 영역(127이하)은 그대로 1byte의 동일값을 사용한다. 한글은 주로 3byte로 표현한다. 인터넷 사이트에서 가장 많이 쓰이는 인코딩으로 엔디안에 상관없이 똑같이 읽을 수 있다는 장점이 있다. 또한 리눅스나 maxos에서는 운영체제 자체의 인코딩 형식을 UTF-8로 통일했다.(window는 레거시 호환을 위해 UCS-2와 병행)

### 4. UCS-2 (Universal Character Set)  
 유니코드 이전에 사용된 국제 인코딩 규격으로 2byte로 이루어져 있다. 총 65,536개의 영역을 사용하고 이 영역을 BMP(기본 다국어 평면)이라고 한다. ASCII 영역(127이하) 문자들도 각각 2byte로 표현되므로 변환없이는 영문자도 호환되지 않는다.
 
### 5. UTF-16  
 가변길이 유니코드 인코딩(2byte, 4byte)으로 2byte영역(BMP영역)까지는 UCS-2와 동일하나, 가변길이 부호화를 통해 4byte영역까지 확장함으로써 더 많은 문자를 표현할 수 있다. ASCII 영역(127이하) 문자들도 각각 2byte로 표현되므로 변환없이는 영문자도 호환되지 않는다.

유니코드란
=============
***
- 유니코드: 전 세계의 모든 문자를 다루도록 설계된 표준 문자 전산 처리 방식(키와 값이 1:1로 매핑된 형태의 코드)

- 유니코드는 하나의 코드셋으로 모든 문자를 나타내야하고, 가변길이라는 특성때문에 특별한 처리를 거쳐 인코딩되게 된다.

- EUC-KR과 차이점 : 예를 들어 **EUC-KR**로 **"안녕"**을 나타내면 **"BE C8(안) B3 E7(녕)"**인데 만약 이렇게 인코딩된 파일을 사용자가 실수로 **EUC-JP**로 디코딩한다면 **BE C8 B3 E7**에 해당하는 일본어(照括)로 출력이 된다. 하지만 유니코드는 전세계의 모든 문자가 unique한 값을 가지기 때문에 UTF8-KR, UTF8-JP같은 개념이 없고 모든 나라의 문자가 **UTF-8**로 인코딩/디코딩 될 수 있다. 

### 유니코드 인코딩 방법(아래의 표 참고)
- **"안녕"** 이라는 한글 문자열이 있을때 유니코드표를 보면 **'안'=C548**, **‘녕'=B155**이다.
- **“C5 48(안) B1 55(녕)”**를 **UTF-**8로 인코딩하게 되면 000800-00FFFF영역이므로 3+3 = 6byte가 되며 해당 값은 **“EC 95 88(안) EB 88 95(녕)”** 이다
**“C5 48(안) B1 55(녕)”**를 **UTF-16**으로 인코딩하게 되면 000800-00FFFF영역이므로 2+2 = 4byte가 되며 해당 값은 **“C5 48(안) B1 55(녕)"** 이다

![_config.yml]({{ site.baseurl }}/images/unicode.png)

character encoding 과정
=============
***
 keyboard의 입력부터 terminal에서 출력되기까지 어떤 과정을 거치게 되는지 살펴보자.  
우선 keyboard에서 (한/영)키를 눌러서 한글입력으로 전환한 후에 "안녕"을 입력한 상황을 생각해보자.  
우리가 'ㅇ', 'ㅏ', 'ㄴ', 'ㄴ', 'ㅕ', 'ㅇ'   여섯개의 버튼이 눌리면 메인보드, CPU를 거쳐 kernel에 인터럽트가 보내지게 된다.   

 1. 커널은 해당 인터럽트를 키보드상에서의 **물리적 위치값(key code)**으로 변환해서 X window system으로 보낸다.  
 2. X window system에서는 함께 눌린 modifier key(Ctrl, Shift 등)을 분석해서 key code를 **key symbol**로 변환한다. key symbol은 'ㅅ','ㅆ',  'A’, ‘Left_alt’와 같은 character형태로 표현되는 symbol을 의미한다. 해당 key symbol은 GUI application이나 terminator로 전달되게 된다.   
 3. terminal emulator에서는 해당 key symbol을 받아 설정된 **charset encoding에 맞게 byte값**으로 변환하게 된다. 예를 들어 terminal인코딩이 UTF-8이라면 terminator는 “EC 95 88(안) EB 88 95(녕)” 의 6byte로 변환 후 vim이나 tbsql처럼 terminator위에서 동작하는 application으로 전달된다. gedit과같이 terminal과 별도로 동작하는 application은 (3)의 과정이 application내부에서 일어나게 된다.  
 4. application에 encoding된 byte값이 저장된다.

![_config.yml]({{ site.baseurl }}/images/keyboardenc.png)

locale, vim과 character encoding
=============
***
### 1. 변수 의미 설명
- locale: 사용자의 언어, 국가뿐 아니라 사용자 인터페이스에서 사용자가 선호하는 사항을 지정한 매개 변수의 모임
- LANG: LC_* 환경변수에 대한 default값을 지정. 즉 지정되지 않은 LC_*환경변수에 대해서 LANG값이 설정됨. LC_*이 설정되면 LANG셋팅을 오버라이딩함.
- LC_ALL: LC_*환경변수 일괄 setting
- terminal encoding: terminal옵션 들어가서 셋팅해주는 charset encoding
- vim의 encoding: vim 자체의 인코딩, 즉 터미널로부터 전달받은 character들을 vim은 자신의 인코딩으로 해석하여 받아들이게 된다. 
- vim의 fileencoding: file로 저장할 때 인코딩. 터미널에서 file -Bi file_name해보면 나오는 file의 인코딩을 결정한다.
- vim의 termencoding: terminal로부터 전달받은 character들의 현재 인코딩. terminal의 인코딩을 vim에서 셋팅해주는게 아니라 단지 알려주는 역할을 할 뿐이다.
- vim에 쓰여지는 character의 byte값은 locale이 아니라 terminal의 encoding을 따른다.

### 2. vim 테스트
vim열고 안녕을 입력후 닫은 후 hexdump값을 본 뒤 다시 열어서 글자가 깨지는지 확인한다.
{% highlight language %}
[상황1]terminal encoding: UTF8, vim enc: EUCKR, vim tenc, fenc = default  
- 입력화면: 깨짐
- hexdump: EC 95 88 EB 88 95(UTF8)
- vim다시열면 enc, fenc가 utf8로 변경돼있고 글자가 제대로 보임.
{% endhighlight %}
{% highlight language %}
[상황2]terminal encoding: UTF8, vim enc: EUCKR, vim tenc=UTF8, fenc = default  
- 입력화면: 잘보임
- hexdump: EC 95 88 EB 88 95(UTF8)
- vim다시열면 enc, fenc가 utf8로 변경돼있고 글자가 제대로 보임.
{% endhighlight %}
{% highlight language %}
[상황3]terminal encoding: UTF8, vim enc: EUCKR, vim tenc=EUCKR, fenc = default  
- 입력화면: ????(물음표값이 입력됨)
- hexdump: 3f 3f 3f 3f (?의 아스키값)
- vim다시열면 enc, fenc가 utf8로 변경돼있고 글자는 여전히 ????로 보임.
{% endhighlight %}
### 3. 결론
- 가능하면 vim의 enc와 terminal의 encoding이 동일해야 한다. 둘이 다를 경우 vim의 tenc를 treminal encoding값으로 셋팅해줘야 글자가 정상적으로 보인다. vim의 tenc와 실제 terminal인코딩이 다를경우 ?가 저장된다.

클라이언트와 character encoding
=============
***
### 1. client charset
- tibero client의 charset을 나타내며 default는 MSWIN949다. client charset을 설정하는 방법은 두가지가 있는데 첫번째는 OS 환경변수 셋팅하는 방법이 있다. 

![_config.yml]({{ site.baseurl }}/images/export_nls.png)

- 두번째로는 tbdsn.tbr에 TB_NLS_LANG=EUCKR 로 셋팅해주는 방법이 있다. tbdsn.tbr의 설정이 OS 환경변수 설정을 오버라이딩한다.  
client driver는 TB_NLS_LANG으로 셋팅된(없으면 default값) charset으로 character를 인식하며, server에 메시지를 보내거나 받을때 해당 charset을 기준으로 server charset과 변환이 일어난다. 참고로 TB_NLS_LANG이 유효하지 않은 값이면 tbsql 로그인할때 error가 발생한다.

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

wide character란
=============
***
### 1. 기본 개념
-  전통적인 8비트 문자 보다 크기가 더 큰 컴퓨터 문자의 자료형으로 window의 MSVC(MS visual c++)에서는 내부적으로 unsigned short형을 사용하며, linux의 gcc는 int형을 사용한다. 
- UCS를 만들기 시작하면서 8비트보다 큰 값을 이용하여 인코딩되는 문자를 저장할 자료형이 필요하게 돼서 개발됐다.
- C/C++ 소스 파일은 보통 US-ASCII나 ISO-2022에 따르는 MBCS(multibyte character set)로 작성된다. (최근에는 UTF-8로도 작성한다.) C/C++컴파일러는 소스파일을 그대로 읽어들이기 때문에 특별한 표식이 없으면 해당 문자열을 유니코드로 인식할 수 없다. 이 문제를 해결하기 위해 c표준에서는 상수 앞에 "L"을 붙여서 문자열을 유니코드로 인식한 후 wchar_t 타입의 문자열로 저장하도록 컴파일러에 요구한다.
- wide char자료형에는 보통 unicode값이 들어가지만 사용자가 강제로 EUCKR이나 UTF8로 인코딩된 값을 넣어줄 수도 있긴하다(컴파일 과정에서 warning발생)

### 2. wide character 테스트 
vim 열어서 c source file에 wchar_t text[] = L”안녕"을 입력하고 text에 저장된 값을 확인해보면, (참고로 '안'의 유니코드 값은 C5 48이고 ‘녕'의 유니코드 값은 B1 55 이다)  

#### [상황1] OS: linux(little endian), terminal encoding:UTF8, locale:UTF8  

 48 C5 00 00 55 B1 00 00

문자열을 UTF8로 인코딩하지 않고 유니코드값을 integer로 인식한 후 endian에 맞게 4 byte wchar_t자료형에 담긴것을 확인할 수 있다.  

#### [상황2] OS: linux(little endian), terminal encoding:EUCKR, locale:EUCKR, vim file encoding=EUCKR  
 
 error: converting to execution character set.  

문자열을 유니코드로 인식하고자 했지만 유효한 값이 아닌 EUCKR로 인코딩된 값이라서 컴파일 에러가 난다.  만약 vim의 file encoding이 UTF8이라면 상황1과 동일하게 정상동작한다.    
즉 L이 붙은 문자열에 대해서는 컴파일러는 해당 환경의 인코딩이 아니라 유니코드로 인식하려고 하며 유니코드 계열의 인코딩이 아니라면 컴파일 에러가 난다.  

### 3. 클라이언트와 wide char
- wide character에는 환경의 인코딩에 상관없이 유니코드 값이 숫자값 그대로 담긴다. 예를 들어 client charset이 UTF8이고 server charset이 EUCKR인 경우 wide character형 문자열("안녕")이 들어온다면 8 byte공간에 

 48 C5 00 00 55 B1 00 00

이 담기게 되고 linux(wchar가 4byte), little endian(뒤쪽바이트부터 읽어야함)을 고려하여 최종적으로 server에서는 4byte의

 BE C8 B3 C7 (euckr로 인코딩된 "안녕")

형태로 저장되게 된다.
