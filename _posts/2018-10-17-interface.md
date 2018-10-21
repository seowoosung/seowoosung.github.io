---
layout: post
title: 객체지향 용어설명(인터페이스, 추상클래스) 
date: 2018-10-17
categories: [ETC]
---

개발을 하다보면 새로운 개념이나 용어를 자주 접하게 된다. 

그럴때마다 책을 찾아보거나 구글검색을 통해 해당 내용을 이해하곤 했다.

하지만 객체지향 용어는 유난히 그 의미가 마음에 와닿지 않았다.  

다행히 C++과 JAVA를 이용해 개발을 하다보니 이러한 용어들에 대한 대략적인 감(?)을 잡을 수 있었다. 

이번 포스트에서는 이러한 감을 토대로 실제 개발상황 예시를 통해 객체지향 용어들을 설명하고자 한다. 

## Concrete Class(구상클래스)
구상클래스는 가장 일반적인 클래스를 의미하며 실제 객체를 생성할 수 있다. 

JAVA에서는 abstract 메서드 없이 class AAA 로 정의한다.

C++에서는 class AAA로 정의하고 순수가상함수(virtual func() = 0;)가 없는 클래스를 의미한다. 

참고로 JAVA에서 abstract와 C++에서 순수가상함수는 동일한 기능이다.

사실 이러한 구상클래스만으로도 우리는 충분히 객체지향 프로그래밍을 할 수 있다. 상속과 오버라이딩, 캡슐화 등등...

하지만 어떤 클래스든 상속받을 수 있고, 또한 상속을 당할 수 있기 때문에 의도치 않은 상황이 발생할 수 있다. 

부모클래스는 '사람'을 정의했지만 이를 상속받은 자식클래스는 '책상'이 돼버릴 수도 있는것이다. 

이러한 상황을 미리 방지하고 표준화된 프로그래밍 환경을 조성하고자 만든 기능이 JAVA의 인터페이스와 추상클래스가 아닌가 생각한다.

## Interface(인터페이스)
프로그래밍에서 일반적으로 말하는 인터페이스란, 컴퓨터 시스템끼리 정보를 교환하는 공유 경계를 의미한다. 

즉 둘 사이에 서로 통신할 수 있는 일종의 통로역할을 한다고 볼 수 있다.

예를 들어 calculator라는 library를 제공하는 제공자가 있다고 하자. 제공자는 library에 Sum이라는 함수를 구현해놓았다.   
{% highlight c++ %}
#include <iostream>

int Sum(int a, int b) {
 return a + b;
}
//calculator.h
{% endhighlight %}

사용자는 calculator library를 이용해 본인의 프로그램을 작성하려고 할 것이다. 

{% highlight c++ %}
#include <iostream>
#include "caculator.h"

void main() {
 int a = 10;
 int b = 10;
 std::cout << Sum(a, b) << std::endl;
}
{% endhighlight %}

여기서 ***Sum(a,b)*** 을 하나의 인터페이스라고 볼 수 있다. 물론 이는 되게 일반적인 의미에서의 인터페이스를 말한다.

그렇다면 JAVA의 인터페이스는 어떤 의미일까? 

JAVA에서는 추상적인 의미가 아니라 실제 코드상에서 interface라는 기능을 제공하고 있다.

실생활에서 있을법한 상황과 JAVA 코드를 통해 살펴보자.

스마트워치를 만들어서 파는 A라는 회사가 있다고 가정하자. 

A회사의 제품은 소프트웨어적으로 완전하지 않아서 B와 C회사에서 해당 제품을 구입한 후에 추가적인 개발을 거쳐 시장에 내놓는다.

A회사의 SmartWatch에는 Alarm과 PowerOn, PowerOff라는 세가지 기능밖에 없다고 한다.

A회사는 Alarm, PowerOn, PowerOff이라는 이름의 인터페이스만 만들어두고 실제 기능은 B와 C회사에서 구현하라는 식으로 제공하게 된다.

이러한 상황에서 A회사의 코드는 아래와 같다.
{% highlight java %}
interface ISmartWatch {
    public void Alarm(int hour) ;
    public void PowerOff();
    public void PowerOn();
}
{% endhighlight %}

A회사의 스마트 워치는 사용자가 종료버튼을 누르는 등의 이벤트가 발생할 때 적절한 인터페이스를 호출할 것이다.

물론 그 인터페이스를 통해 실제로는 상속받아 구현된 method를 호출하게 될텐데 그 부분에 대한 것은 아래와 같다.

B, C 회사는 위의 코드를 상속받아 본인만의 시계를 만들게된다. B나 C회사의 코드는 아래와 같다.

{% highlight java %}
class GalaxyWatch implements ISmartWatch {
    public void Alarm(int hour) {
     System.out.print("GalaxyWatch의 알람이 울렸습니다!!");
    }
    public void PowerOff() {
     System.out.print("GalaxyWatch가 종료됩니다");
    }
    public void PowerOn() {
     System.out.print("GalaxyWatch가 켜집니다");
    }
}
{% endhighlight %}

종료 버튼이 눌렸을 때 PowerOff라는 ISmartWatch의 함수가 호출되더라도 실제로는 GalaxyWatch의 PowerOff가 호출될 것이다. 

위의 예제에서 인터페이스는 어떤 겉모습을 미리 정해두고 구현은 다른 사람에게 맡기는 용도로 사용되고 있다.

어떠한 코드에 접근할 수 있는 겉모습을 정의한다는 의미에서 볼 때 위에서 말한 일반적인 의미의 인터페이스로서 역할을 하는 것 같아보인다.

하지만 위의 상황은 구상클래스로도 동일하게 구현할 수 있기때문에 아직 interface가 특별한 기능같아 보이진 않는다.

그렇다면 JAVA의 interface는 어떤점이 특별한 것일까?

JAVA에서 interface의 특별함은 구현을 강제시킨다는 점에 있다. 

위와같은 상황에서 B, C회사가 Alarm과 PowerOn/Off라는 method를 오버라이딩 하지 않았다면 컴파일중 에러가 발생했을 것이다. 

A회사는 자신이 의도한대로 B, C 회사가 Alram과 PowerOn/Off를 구현해야만하는 상황을 만든것이다.

위와 같은 상황 말고도 어떤 프로젝트를 개발하기 전에 interface를 먼저 설계하고 interface대로 테스트 코드를 작성한 다음, 

하나하나씩 interface를 상속받아 실제 기능을 구현해 나가는 것도 JAVA interface활용의 한 예시라고 볼 수 있다. 

또는 부모클래스로 인터페이스를 둠으로써 자신이 정의한 클래스의 멤버변수나 내부 함수를 감추고자 할때도 유용하게 사용될 수 있다.

> 참고로 C++은 interface라는 기능이 없다. 하지만 모든 method를 순수가상함수로 선언함으로써 JAVA의 interface와 동일한 역할을 수행 할 수 있다.

## Abstract Class(추상클래스)

추상클래스는 보통 인터페이스와는 다른 용도로 사용된다.

JAVA의 interface가 본인이 가진 모든 method들의 구현을 자식클래스에게 강제한다면,

추상클래스는 특정한 method의 구현만을 강제한다.

예시를 통해 알아보자. 앞선 A,B,C 회사의 예시를 조금 변형시켜볼 것이다.

A회사는 몇년동안 SmartWatch를 만들다보니 기본적인 소프트웨어를 구현할 수 있는 기술력도 갖추게 됐다.

A회사는 B, C회사가 좀더 편하게 자사의 제품을 사용할 수 있도록 PowerOn, PowerOff는 직접 개발하기로 했다.

이제 기존에 제공하던 interface를 상속받은 추상클래스를 B, C회사에게 제공할 것이다.

이러한 상황에서 A회사의 코드는 아래와 같다.

{% highlight java %}
abstract class SmartWatch implements ISmartWatch {
    public abstract void Alarm(int hour) ;
    public void PowerOff() {
     System.out.print("SmartWatch가 종료됩니다");
    }
    public void PowerOn() {
     System.out.print("SmartWatch가 켜집니다");
    }
}
{% endhighlight %}

B, C회사는 더이상 PowerOn과 PowerOff를 본인들이 구현할 필요가 없어졌다. 

이제 abstract로 정의된 Alarm만 본인들이 직접 구현하면된다.

B나 C회사의 코드는 아래와 같다
{% highlight java %}
class GalaxytWatch extends SmartWatch {
    public abstract void Alarm(int hour) {
     System.out.print("Galaxy의 Alarm이 울렸습니다");
    }
}
{% endhighlight %}

이제 PowerOn과 PowerOff에 대해서는 A회사가 제공하는 method를 호출하게 되고 Alram만 본인들의 method를 호출하게 되는것이다.

추상클래스는 이처럼 자식클래스에서 부모클래스의 특정 기능만을 확장시키고자 할때 유용한 기능이다.

본 예시에서는 추상클래스가 interface를 상속받았지만 꼭 그럴필요는 없다.

또한 본 예시에서는 추상클래스가 마치 interface를 발전시킨 모습으로 묘사됐지만 그건 아니다. 

차이점을 보여주기 위해 둘을 연관지어봤을뿐 둘은 서로 독립적인 기능이다.

## 결론

인터페이스와 추상클래스를 구분하는 것은 정말 어려운 것 같다. 이는 인터페이스와 추상클래스를 너무 명확하게 구분짓고자 했기 때문일지도 모른다. 
 
사실 두가지 기능은 특정 method의 구현을 강제한다는 기능적인 측면에서는 거의 비슷하다고 볼 수 있다. 
 
전체 method인지(인터페이스) 부분적인 method(추상클래스)인지에 대한 차이만 있을뿐이다. 

> 심지어 C++에는 interface라는 기능이 따로 없다. 대신 순수가상함수를 적절히 사용해서 JAVA의 interface나 추상클래스와 동일한 기능을 구현할 수 있다. 
 
그렇다면 기능적인 측면 말고 다른 측면에서의 차이점은 무엇이 있을까?

가장 큰 차이점은 그 용도에서 찾을 수 있다.(개인적인 의견임)

인터페이스는 자식클래스가 자신을 상속받아 구현해야할 기능들을 정의해주고, 
 
추상클래스는 자식클래스가 자신을 상속받아 특정 기능만을 확장시켜 사용할 수 있도록 도와주는...?    
 
이 또한 애매모호 하지만, 인터페이스와 추상클래스의 차이점은 그 용도에서 찾는것이 그나마 좀더 명확해 보인다. 
 
다만 용도라는 것이 사용하는 사람에 따라 달라질 수 있기 때문에 두가지 개념의 구분이 어려운건 아닐까...
 
정리해봐도 여전히 어려운 것 같다.
