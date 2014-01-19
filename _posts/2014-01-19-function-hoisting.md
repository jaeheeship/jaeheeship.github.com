---
layout: post_page
title: "자바스크립트의 함수 hoisting"
description: "자바스크립트의 함수 hoisting에 대해서 알아보자"
category : "javascript"
tags: [javascript]
---

자바스크립트에서 함수가 차지하는 영역은 매우 중요하며, 함수의 동작을 제대로 이해해야지만 모듈러 패턴이나 프로토타입을 활용한 상속등을 이해할 수 있다. 오늘은 가볍게 함수 호이스팅에 대해서 정리해본다. 
자바스크립트 변수는 block scope가 없으며 , function scope만이 존재한다. 즉 함수안에서 선언된 변수는 함수내의 어디에서나 접근 가능하다. 


아래의 예시를 보자.
{% highlight javascript  %}

function test(){
    var hello = 'hello' ,
        pass = true ;
    
    if(pass){
        for(var i = 0 ; i < 3 ;i++){
            console.log(i) ; //0,1,2  가 출력된다.
        }
    }

    console.log(i) ; //3이 출력된다.
} 
{% endhighlight javascript %}

위의 예제에서 보는 것처럼 ***if문 block안에서 i라는 변수를 선언했지만 if문 block 밖에서도 접근이 가능하다.*** 즉 자바스크립트 함수내에서 선언된 변수는 그 변수가 선언된 함수내에서 어디서나 접근이 가능하다. 이 개념을 우선 머리속에 넣어두고 다음 예제를 살펴보자. 

{% highlight javascript  %}
var global = "I'm a global variable" ; 

function hoisting(){ 
    console.log(global) ; //1번 출력
    var global  = "I'm not a global variable" ;
    console.log(global) ; //2번 출력
} 

hoisting() ; 
console.log(global) ; //3번 출력
{% endhighlight javascript %}

위의 예제의 출력결과는 어떻게 될까? 

>1번 출력 : undefined <br/>
>2번 출력 : I\'m not a global variable<br/>
>3번 출력 : I\'m a global variable

1번 출력에서 I\'m a global variable이 출력될거라 생각했을지도 모른다. 하지만 다시 앞서 말한부분을 상기시켜보자. 
여기서 잠깐 3번출력에 대해서 의문이 있다면 지금은 간단하게 생각하자. \'var\' keyword가 붙은 변수는 local 변수이며 , global 변수와 이름이 같을때는 local 변수를 우선시 한다고 정리하자.

>함수내에서 선언된 변수는 그 변수가 선언된 함수의 내부 어디서나 접근이 가능하다.

자바스크립트의 함수 동작을 다시 한번 상기시켜볼 필요가 있다. ***자바스크립트는 함수내의 선언된 변수를 모두 함수의 최상단으로 모두 올려 놓는다. 이것을 호이스팅(hoisting)이라고 하는데*** , 이 함수 호이스팅 때문에 위와 같은 결과가 나온것이다. 
즉, 위의 예제를 자바스크립트가 동작하는대로 다시 풀어보면 , 

{% highlight javascript  %}
var global = "I'm a global variable" ; 

function hoisting(){ 
    var global ; 
    console.log(global) ; //1번 출력
    global  = "I'm not a global variable" ;
    console.log(global) ; //2번 출력
} 

hoisting() ; 
{% endhighlight javascript %}

이렇게 동작하는 것과 동일하다. 

그래서 자바스크립트에서는 ***single var pattern***이라고 해서 

{% highlight javascript  %}

function example(){ 
    var i = 0 , 
        arr = [] ,
        hello = 'hello' ,
        num = 123 ,
        isNull = true ; 

    console.log(i) ; 
} 

{% endhighlight javascript %}

이런식으로 함수의 최상단에 하나의 var로 함수내에서 사용되는 변수들을 선언하고 값을 셋팅하는 것을 추천한다.
