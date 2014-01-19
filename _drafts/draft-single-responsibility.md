---
layout: post_page
title: "SOLID Principle 정리 - 01 [ Single Respoinsibility]"
description: ""
category: "php"
tags: [namespace]
--- 

Object Oriented Design 을 공부할때 항상 나오는 것이 SOLID 이다. 본인은 최근에서야 공부하게 되었다. 보는 책마다 SOLID에 대한 얘기를 하길래 무엇인가 했다. ( 프로그래밍을 한지 몇년째인데 이제서야 접했다는 것이 부끄러울 뿐이다. ) 
OOP언어를 사용한다고 해서 객체지향적으로 프로그래밍을 한다는 것은 아니다. OOP언어라는 것은 OOP를 명료하게 프로그래밍 할 수 있다는 것이지 , 설계를 책임져주는 것은 아니다. OOP언어로 코딩을 하지만, 코딩하는 사람에 따라서 결과물이 절차지향일 수 도 있다. 그러므로 우리는 클래스를 설계하는 것을 공부해야 한다. 

클래스가 잘 설계됬는지의 판단 기준은 아래의 SOLID Principle을 상기하면 된다. 

SOLID Principle은 객체지향 디자인을 할때 권장하는 원칙이다. ( 권장이라고 하는 것은 상황에 따라 깨뜨릴수도 있으므로 , 절대적인 것은 없다) 

* Single Responsibility Principle
* Open Closed Principle
* Liskov Substitution Principle
* Interface Segregation Principle
* The Dependency Inversion Principle 

### Single Responsibility Principle 

오늘 설명할 내용은 Single Responsibility 이다. Single Responsibility 는 하나의 클래스는 오직 단 하나의 일만 해야 한다는 것이다. 이 원칙을 머리속에 두고 설계를 하면 , 클래스가 무거워지는것을 막아준다. 클래스를 쓴다고 해서 객체지향 프로그래밍을 하는 것은 아니다.  클래스가 잘 설계되어 있는지는 저 위의 5가지 원칙을 상기시키면 된다. SOLID Principle 은 서로가 연관되어 있는 것이라 저 위의 원칙중 하나가 깨지면 다른 원칙도 깨지는 것이 일반적이다. 



{% highlight php linenos %}
{% endhighlight php %}
