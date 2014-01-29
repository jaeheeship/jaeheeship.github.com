---
layout: post_page
title: "함수 정의에 관해서"
description: "자바스크립트는 함수정의 방법이 두가지가 있다. 하나는 function declaration 이고 다른하는 function expression 이다. 이 두가 정의 방법에 대해서 알아보고자 한다."
category : "javascript"
tags: [javascript]
---
코뿔소 책으로 유명한 자바스크립트 명저, 자바스크립트 완벽가이드에는 함수 설명에 꽤 많은 페이지를 할당하고 있다. 그만큼 중요하며, 자바스크립트를 이해하는데 많은 내용이 포함되어 있다. 처음 볼때는 놓치는 부분이 많았는데 , 두번째 볼때 보이는 내용은 사뭇다르다. 책에 나와있던 내용을 바탕으로 자바스크립트 함수정의에 대해서 알아본다. 

자바스크립트에서 함수를 정의하는 방법은 두가지가 있다. 

**Function declaration** 함수선언문이라 불리며 다른 프로그래밍 언어를 접했다면 자주 볼 수 있음직한 문장이다.

{% highlight javascript %}
function func(){
}
{% endhighlight %}

**Function Expression** 일반적으로 함수표현식이라 불리며 자바스크립트에서 자주 볼 수 있다.
함수표현식은 함수를 값으로 사용할 수 있는 것을 의미한다.
> A function definition expression defines a JavaScript function , and the value of such an expression is the newly defined function . -- javascript definitive guide 6th.
{% highlight javascript %}
var func = function (){
}
{% endhighlight %}

이렇게 두가지이다. 간단하다고 하면 간단하지만, 각각의 차이점이 무엇인지 인지 못하면 자바스크립트에서 발생하는 오류를 잡는데 오랜시간이 걸릴지도 모른다. 

전자의 Function Declaration은 function이라는 키워드뒤에 선언되는 함수를 식별할 수 있는 식별자(identifier), 파라미터가 뒤 따라오게 된다.

후자의 Function Expression은 변수에 할당해서 사용하는 것을 얘기하는데, 함수의 내용만 봐서는 두가지 방법이 별 차이가 없어보인다. 그 차이점은 예시를 통해서 알아보고자 한다. 

### 함수선언문(Function Declaration) 부터 알아보자.

{% highlight javascript %} 
//함수선언문을 이용한 예제
hello();

function hello(){
  alert("hello");
}
{% endhighlight %}

hello를 화면에 나타내는 간단한 내용이다. 

이부분에서 알아야 되는 부분이 ***호이스팅(hoisting)***이다. ***함수선언문은 자바스크립트 엔진이 스크립트의 최상단 또는 다른 함수안에서 정의된 함수같은 경우 그 정의된 함수의 최상단으로 끌어올린다.*** 무슨 말이냐 하면

{% highlight javascript %} 
function hello(){ //hoisting된 함수 선언문.
  alert("hello");
}

hello(); 
{% endhighlight %}
이처럼 동작을 하는 것을 의미한다. 변수의 hosting과 비슷하지만, ***문장 전체가 호이스팅 된다는 것이 다르다***. 이 함수선언문이 호이스팅 되기 때문에, ***함수 선언문으로 선언된 함수는 함수가 선언된 영역 어디에서든 접근 할 수 있다***(단 if문안에서나 while문 안에서는 호출을 못할 수도 있다, ***하지만 자바스크립트 엔진을 구현한 브라우저에 따라 if문안에서도 함수선언문으로 정의된 함수일지라도 호출할 수 있다.***)
***실제로 함수선언문은 변수를 선언하고 나서 그 변수에 함수를 할당하는것과 같다***


### 함수표현식(Function Expression)을 알아보자.

{% highlight javascript %} 
//함수 표현식을 이용한 예제
hello();

var hello = function(){
  alert("hello");
}
{% endhighlight %}

함수표현식을 이용한 예제는 결과가 어떨까? 오류가나며 화면에 어떠한 것도 출력이 되지 않는다.

***오류가 나는 이유는 변수의 호이스팅 때문이다.*** 자바스크립트 엔진은 변수를 호이스팅 한다. 즉 변수를 그 변수가 선언된 영역의 최상단으로 끌어올린다.
즉 위의 예제는 아래처럼 동작하는 것으로 이해해볼 수 있다.

{% highlight javascript %} 
var hello ;
hello();

hello = function(){
  alert("hello");
}
{% endhighlight %}

이렇게 소스를 이해한다면 오류가 나는 것은 분명하다. 먼저 hello라는 변수를 선언하고 그 변수에 아무런 값도 할당되지 않았기 때문에 첫번째 줄의 hello는 undefined상태이다. 그러므로 당연히 두번째 문장에서 undefined상태인 hello를 호출하려고 하니 에러가 나는 것이다. 

> 함수선언문의 호이스팅과 변수의 호이스팅의 차이점을 알아둘 필요가 있다.

####### 같이보면 좋아요.

* [자바스크립트의 호이스팅](/javascript/2014/01/19/javascript-hoisting.html)
