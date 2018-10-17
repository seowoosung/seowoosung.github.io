---
layout: post
title: 객체지향 용어설명 
date: 2018-10-17
categories: [ETC/OOP]
---

객체지향 용어는 정확히 이해하기 정말 어려운것 같다.

개발을 하다보면 수많은 새로운 개념이나 용어를 접하게 된다. 그런데 유난히 객체지향 용어는 마음에 딱 와닿지 않았다.  

다행히도 C++과 JAVA를 이용해 개발을 하다보니 이러한 용어들에 대한 대략적인 감(?)을 잡을 수 있었다. 

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

사용자는 calculator library를 이용해 보인의 프로그램을 작성하려고 할 것이다. 사용자의 코드는 아래와 같다.

{% highlight c++ %}
#include <iostream>
#include "caculator.h"

void main() {
 int a = 10;
 int b = 10;
 std::cout << Sum(a, b) << std::endl;
}
{% endhighlight %}

인터페이스라는 기능은 JAVA에만 있고 C++에는 없다.




## Abstract Class(추상클래스)

## Concrete Class(구상클래스)

## is-a와 has-a

