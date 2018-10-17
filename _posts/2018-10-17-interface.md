---
layout: post
title: 객체지향 용어설명 
date: 2018-10-17
categories: [ETC/OOP]
---

개발을 하다보면 수많은 새로운 개념이나 용어를 접하게 된다. 그런데 유난히 객체지향 용어는 마음에 딱 와닿지 않았다.  

다행히 C++과 JAVA를 이용해 개발을 하다보니 이러한 용어들에 대한 대략적인 감(?)을 잡을 수 있었다. 

이번 포스트에서는 이러한 감을 토대로 실제 개발상황과 연관지어서 객체지향 용어들에 대해 설명하고자 한다. 

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

여기서 ***Sum(a,b)*** 을 하나의 인터페이스라고 볼 수 있다. 물론 이 예제에의 인터페이스는 되게 추상적인 인터페이스를 의미한다.

그렇다면 JAVA에서 말하는 인터페이스는 어떤것을 의미할까? 

JAVA는 추상적인 의미가 아니라 실제 코드상에서 인터페이스를 구현할 수 있도록 그 기능을 제공하고 있다.

JAVA 코드를 통해 살펴보자

{% highlight java %}
interface Calculatable {
    public void setOprands(int first, int second, int third) ;
    public int sum(); 
    public int avg();
}
{% endhighlight %}




## Abstract Class(추상클래스)

## Concrete Class(구상클래스)

## is-a와 has-a

