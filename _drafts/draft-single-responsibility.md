---
layout: post_page
title: "SOLID Principle 정리 - 01 [ Single Respoinsibility]"
description: ""
category: "php"
tags: [namespace]
--- 

Object Oriented Design 을 공부할때 항상 나오는 것이 SOLID 이다. 본인은 최근에서야 공부하게 되었다. 보는 책마다 SOLID에 대한 얘기를 하길래 무엇인가 했다. ( 프로그래밍을 한지 몇년째인데 이제서야 접했다는 것이 부끄러울 뿐이다. ) 

SOLID Principle은 객체지향 디자인을 할때 권장하는 원칙이다. ( 권장이라고 하는 것은 상황에 따라 깨뜨릴수도 있으므로 , 절대적인 것은 없다) 

* Single Responsibility Principle
* Open Closed Principle
* Liskov Substitution Principle
* Interface Segregation Principle
* The Dependency Inversion Principle 

### Single Responsibility Principle 

오늘 설명할 내용은 Single Responsibility 이다. Single Responsibility 는 하나의 클래스는 오직 단 하나의 일만 해야 한다는 것이다. 이 원칙을 머리속에 두고 설계를 하면 , 클래스가 무거워지는것을 막아준다. 클래스를 쓴다고 해서 객체지향 프로그래밍을 하는 것은 아니다. OOP 언어라는 것은 OOP를 명료하게 할 수 있다는 것이지, 설계를 책임져주는 것은 아니다. 

{% highlight php linenos %}
{% endhighlight php %}
