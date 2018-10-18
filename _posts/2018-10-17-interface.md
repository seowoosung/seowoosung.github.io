---
layout: post
title: 객체지향 용어설명 
date: 2018-10-17
categories: [ETC/OOP]
---

개발을 하다보면 수많은 새로운 개념이나 용어를 접하게 된다. 그런데 유난히 객체지향 용어는 마음에 딱 와닿지 않았다.  

다행히 C++과 JAVA를 이용해 개발을 하다보니 이러한 용어들에 대한 대략적인 감(?)을 잡을 수 있었다. 

이번 포스트에서는 이러한 감을 토대로 실제 개발상황과 연관지어서 객체지향 용어들에 대해 설명하고자 한다. 

## Concrete Class(구상클래스)
JAVA에서 abstract 메서드 없이 class AAA 로 정의한, C++에서는 class AAA로 정의하고 순수가상 method(virtual func() = 0;)가 없는 클래스를 의미한다. 구상클래스는 실제 객체를 생성할 수 있다. 

구상클래스만으로도 충분히 객체지향 프로그래밍을 할 수 있다. 상속과 오버라이딩, 캡슐화 등등...

하지만 누구나 상속받을 수 있고, 상속을 당할 수 있기 때문에 의도치 않은 상황이 발샐할 수 있다. 

부모클래스는 '사람'을 의도했지만 이를 상속받은 자식의 자식클래스는 '책상'이 돼버릴 수도 있는것이다. 

이러한 상황을 미리 방지하고 표준화된 프로그래밍 환경을 조성하고자 만든 기능이 JAVA의 인터페이스와 추상클래스가 아닌가 생각한다.

## Interface(인터페이스)
인터페이스는 제공자와 사용자가 서로 통신할 수 있는 유일한 통로 역할을 한다.

예를 들어 calculator라는 library를 제공하는 제공자가 있다고 하자. 제공자는 library에 Sum이라는 함수를 구현해놓았다.   
{% highlight c++ %}
#include <iostream>

int Sum(int a, int b) {
 return a + b;
}
//calculator.h
{% endhighlight %}

사용자는 calculator library를 이용해 보인의 프로그램을 작성하려고 할 것이다. 

{% highlight c++ %}
#include <iostream>
#include "caculator.h"

void main() {
 int a = 10;
 int b = 10;
 std::cout << Sum(a, b) << std::endl;
}
{% endhighlight %}

여기서 ***Sum(a,b)*** 을 하나의 인터페이스라고 볼 수 있다. 물론 이 예제에서는 되게 추상적인 인터페이스를 의미한다.

그렇다면 JAVA에서 말하는 인터페이스는 어떤것을 의미할까? 

JAVA는 추상적인 의미가 아니라 실제 코드상에서 인터페이스를 구현할 수 있도록 그 기능을 제공하고 있다.

실생활에서 있을법한 상황과 함께 JAVA 코드를 통해 살표보자.

스마트워치를 만들어서 파는 A라는 회사가 있다고 가정하자. A회사의 제품은 소프트웨어적으로 완전하지 않아서 B와 C회사에서 
해당 제품을 구입한 후에 추가적인 개발을 거쳐 시장에 내놓는다.

A회사의 SmartWatch에는 Alaram과 PowerOff, PowerOff라는 세가지 기능밖에 없다고 한다.

A회사는 Alarm, PowerOff, PowerOff이라는 이름의 인터페이스만 만들어두고 실제 기능은 B와 C회사에서 구현하라는 식으로 제공하게 된다.

이러한 상황에서 A회사의 코드는 아래와 같다.
{% highlight java %}
interface ISmartWatch {
    public void Alarm(int hour) ;
    public void PowerOff();
    public void PowerOn();
}
{% endhighlight %}

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

이 상황에서 인터페이스는 어떤 겉모습을 미리 정해두고 구현은 다른 사람에게 맡기는 모습으로 사용되고 있다.

어떠한 코드에 접근할 수 있는 겉모습을 정의한다는 의미에서 볼때 위에서 말한 추상적인 의미의 인터페이스로서의 역할을 하는 것 같아보인다.

하지만 ISmartWatch를 굳이 인터페이스로 정의하지 않고 일반적인 class로 정의해도 똑같은 효과를 얻을 수 있을 것이다.

JAVA에서 interface기능의 역할은 무엇일까?

JAVA에서 interface의 특별함은 구현을 강제시킨다는 점에 있다. 

위와같은 상황에서 B, C회사가 Alarm과 PowerOff라는 method를 오버라이딩 하지 않았다면 컴파일중 에러가 발생했을 것이다. 

A회사는 B, C 회사가 자신이 의도한대로 Alram과 PowerOff를 자기 스스로 구현해야만하는 상황을 만든것이다.

위와 같은 상황 말고도 어떤 프로젝트를 개발하기 전에 interface를 먼저 설계하고 interface대로 테스트 코드를 작성한 다음, 
하나하나씩 interface를 상속받아 구현하는 것도 JAVA interface활용의 한 예시라고 볼 수 있다. 

참고로 C++은 interface라는 기능이 없다. 하지만 모든 method를 순수가상함수로 선언함으로써 JAVA의 interface와 동일한 역할을 수행 할 수 있다.

## Abstract Class(추상클래스)

추상클래스는 보통 인터페이스와는 다른 용도로 사용된다.

JAVA의 interface가 본인이 가진 모든 method들의 구현을 자식클래스에게 강제한다면,

추상클래스는 특정한 method의 구현만을 강제한다.

예시를 통해 알아보자. 앞선 A,B,C회사의 예시를 조금만 변형시켜볼 것이다.

A회사는 몇년동안 SmartWatch를 만들다보니 기본적인 소프트웨어를 구현할 수 있는 기술력도 갖추게 됐다.

A회사는 B, C회사가 좀더 편하게 본인의 제품을 사용할 수 있도록 PowerOn, PowerOff는 직접 개발하기로 했다.

이제 interface가 아닌, 이를 상속받은 추상클래스를 B, C회사에게 제공하게 됐다.

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

이제 B, C회사는 더이상 PowerOn과 PowerOff를 본인들이 구현할 필요가 없어졌다. 다만 abstract로 정의된 Alarm은 본인들이 직접 구현해야한다.(아니면 컴파일 에러가 발생한다)

B나 C회사의 코드는 아래와 같다
class GalaxytWatch extends SmartWatch {
    public abstract void Alarm(int hour) {
     System.out.print("Galaxy의 Alarm이 울렸습니다");
    }
}
{% endhighlight %}

이제 PowerOn과 PowerOff에 대해서는 A회사가 제공하는 method를 호출하게 되고 Alram만 본인들의 method를 호출하게 되는것이다.

추상클래스는 이처럼 자식클래스에서 특정 기능을 상속받아 확장시키고자 할때 유용한 기능이다.

본 예시에서는 추상클래스가 interface를 상속받았지만 꼭 그럴필요는 없다.

또한 본 예시에서는 추상클래스가 마치 interface를 발전시킨 모습으로 묘사됐지만 오해하지 않길 바란다.

차이점을 보여주기 위해 둘을 연관지어봤을뿐 둘은 서로 독립적인 기능이다.

## 결론

 인터페이스와 추상클래스를 구분하는 것은 정말 어려운 것 같다. 이는 인터페이스와 추상클래스를 너무 명확하게 구분짓고자 했기 때문인것 같다. 
 
 사실 두가지 기능은 특정 method의 구현을 강제한다는 기능적인 측면에서는 거의 비슷하다고 볼 수 있다. 
 
 전체 method인지(인터페이스) 부분적인 method(추상클래스)인지에 대한 차이만 있을뿐이다. 심지어 C++에는 interface라는 기능이 따로 없다. 
 
 대신 순수가상함수를 적절히 사용해서 JAVA의 interface나 추상클래스와 동일한 기능을 구현할 수 있다. 
 
 그렇다면 기능적인 측면 말고 그 용도에서의 차이점은 무엇일까?
 
 인터페이스는 자식클래스가 자신을 상속받아 구현해야할 기능들을 정의하고, 
 
 추상클래스는 자식클래스가 자신을 상속받아 특정 기능만을 확장시켜 사용할 수 있도록 도와주는...?    
 
 어찌됐든 인터페이스와 추상클래스의 차이점은 그 용도에서 찾는것이 더 명확한 것 같다. 
 
 다만 용도라는 것이 사용하는 사람에 따라 달라질 수 있기 때문에 두가지 개념의 구분이 어려운것 겉다...
 
 정리해도 어려운것 마찬가지다.
