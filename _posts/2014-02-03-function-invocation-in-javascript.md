---
layout: post_page
title: "자바스크립트의 함수 호출 이해하기"
description: ""
category: "javascript"
tags: [function,javascript]
---

지금까지 경험으로는 자바스크립트를 잘 이해하기 위해서는 기본적인 것이 튼튼해야함을 느꼈다. 현재까지의 자바스크립트 아티클이 함수부분인 이유는 본인이 함수부분을 계속 공부하고 있어서이다. 자바스크립트의 디자인패턴등은 이러한 함수등을 잘 이용하는 것이기 때문에, 함수를 이해하는것 목표로 한다. 이번 글의 주제는 함수호출에 대한 부분인데 , ***함수호출을 통해서 생성되는 execution context를 바르게 이해하고자 한다.***

자바스크립트에서 함수를 호출하는 방법은 크게 네가지이다. 
1. 일반적인 함수 호출 : 우리가 흔히 알고 있는 f() 형식의 함수호출이다.
2. 메소드로의 호출 : 함수가 특정 객체의 property로 존재하며 , o.f()처럼 어떤 객체의 property인 상태로 호출하는 것을 얘기한다.
3. 생성자 함수로의 호출 : new f() 형식의 호출을 의미한다.
4. 간접적인 호출 : apply,call을 이용해서 함수를 호출하는 것을 의미한다.  

***함수는 호출될때마다 execution context라는 객체가 생성이 된다. 이 execution context는 this라는 keyword로 접근 할 수 있으며, 위의 네가지 함수 호출에 따라서 이 execution context의 값은 다르다. 즉 this는 함수정의 시점에 정해지는 것이 아니라, 함수가 실행되는 시점에 정해진다. 이점이 자바스크립트를 이해하기 어렵게 하면서도 자바스크립트의 강력한 무기이기도 하다.***

> execution context는 함수가 실행될때마다 생성되며 , this키워드를 통해서 execution context에 접근 할 수 있다. 즉 executionContext = this 이다.

### 1.일반적인 함수 호출

함수를 호출하는 가장 일반적인 형태이다. 

{% highlight javascript linenos %}
function sum(a,b){
  return a+b; 
}

sum(3,5) ; 
{% endhighlight javascript %}

이런식의 호출을 얘기하며, 여기서 this(execution context)는 javascript 엔진에 따라서 달라진다. ES3(ECMA script 3), ES5의 non-strict-mode에서는 global object(웹브라우저에서는 window)를 가르키며, ES5 strict-mode에서는 undefined이 된다. 이러한 성질을 이용해서 strict mode여부를 판별 할 수 있다.

{% highlight javascript linenos %}
var isStrict = (function(){
  return !this ;
})();

if(isStrcit) {
  alert("Strcit Mode 입니다.");
} else {
  alert("Non Strcit Mode 입니다.");
}
{% endhighlight javascript %} 

함수를 정의하자마자 바로 호출시켰다. 위의 함수 호출은 일반적인 형태이므로 this객체는 window 혹은 undefined를 가르키게 된다. 즉 strict mode인 경우 this의 값은 undefined가 되며 리턴값은 undefined의 not 연산을 통해서 true값을 리턴한다.

> 일반적인 함수호출에서의 this(execution context)는 ECMA SCRIPT 3 와 ECMA SCRIPT 5의 non strict mode에서는 global 객체(웹브라우저라면 window)를 가르키며, ES5의 strict mode에서는 undefined 가 된다.

### 2. 메소드로의 호출

자바스크립트 객체는 key,value로 이뤄진 property들로 이뤄진다. 여기서 property의 value가 함수식일 경우 우리는 이것을 그객체의 메소드라고 부르며 '.' 연산자를 통해서 객체의 메소드에 접근 할 수 있다.

{% highlight javascript lineos %}
var obj = {
  sum : function(a,b){
    this.a = a ;
    this.b = b ;
    return a+b;
  },
  isThis : function(){
    if(this === obj){
      alert('isThis is obj');
    }
  }
}

obj.sum(3,5) ;

if(obj.a == 3){
  alert('obj.a is 3') ; 
}

if(obj.b == 5){
  alert('obj.b is 5') ; 
}

obj.isThis() ; //alert문이 실행된다.
{% endhighlight javascript %}

이렇게 obj.sum(3,5) 형태처럼 호출하는 것을 메소드 호출이라 하며 sum은 obj객체의 메소드가 되는 것이다. ***메소드로 호출될때의 메소드의 this는 메소드와 연결되어 있는 객체를 가르킨다. 즉 여기서는 obj라는 변수가 this가 되는 것이다.***
위에 예제에서 sum이라는 메소드는 this객체에 'a','b'라는 key를 만들고 값을 할당했다. 앞서 말한 것처럼 메소드로 호출될때의 this(execution context)는 메소드가 속해있는 객체를 가르킨다 했으므로, this === obj 이된다.

만약 그러면 아래의 예제의 결과값은 어떻게 될까?

{% highlight javascript lineos %}
var obj = {
  sum : function(a,b){
    this.a = a ;
    this.b = b ;
    return a+b;
  },
  isThis : function(){
    if(this === obj){
      alert('isThis is obj');
    }
  },
  nestedObj : {
    isThis: function(){
      if(this === obj){
        alert('this is obj');
      } else if(this === obj.nestedObj){
        alert('this is nested obj');
      }
    }
  }
} 


obj.nestedObj.isThis() ; // this is nested obj 가 출력된다. 
{% endhighlight javascript %}

제일 마지막 줄에 obj.nestedObj.isThis() 문장을 살펴보자. 위에 예제를 실행시키면 화면에 'this is nested obj' 라는 문구가 출력된다. 메소드로 호출될때 this는 메소드를 호출시킬 당시에 객체를 가르킨다고 했다. 위에서 isThis()라는 메소드를 호출할때 isThis를 갖고 있는 녀석은 nestedObj이므로 this(execution contenxt)는 obj.nestedObj가 된다.

> 다시한번 정리하면, 메소드로 호출될때의 그 메소드가 호출될 당시에 this는 그 메소드를 직접적으로 갖고 있는 객체를 가르킨다. 다시한번 말하지만, this 호출시점과 관련이 있다.

### 3.  생성자 함수로의 호출 

new Constructor 식으로 호출 하는 것을 말하면 자바스크립트에서 정의 된 함수를 new func()식으로 호출하면 새로운 객체가 생성이 되며, 새롭게 생성된 객체는 execution context가 되며 this 키워드를 통해서 접근 할 수 있다.

{% highlight javascript %}
var Sum = function(a,b){ //생성자 함수를 명시적으로 나타내기 위해서 함수의 첫글자는 대문자로 시작한다.
  this.a = a ;
  this.b = b ;
}

var newObj = new Sum(3,5);
var newObj2 = new Sum(3,5); 

if(newObj !== newObj2){
  alert('newObj is not eqaul to newObj2');
}
{% endhighlight javascript %}

생성자 함수는 기존의 메소드 함수나 일반적인 함수호출과는 조금 다른 양상을 보인다. 생성자 함수는 리턴값이 없어도 새롭게 객체를 생성해서 리턴한다. 위에 Sum이라는 생성자 함수는 값을 리턴을 않하지만, newObj에는 새롭게 생성된 오브젝트가 할당된다. 

***또한 new 를 통한 생성자 함수 호출은 매번 새로운 객체(execution context)를 생성시킨다.*** 위의 예제에서 볼 수 있듯이 newObj와 newObj2는 서로 다른 독립된 객체이다.  

> new연산자를 통한 생성자 함수 호출은 매번 새로운 객체를 생성시키며 , 생성된 객체를 리턴한다. 생성자 함수부분에 대해서는 객체 생성 부분에서 다시 다룰 예정이다.

### 4.  Call,Apply를 활용한 호출 

사실상 자바스크립에서 다루는 모든 것은 객체이다. 심지어 함수도 객체이다. 객체라하면 key:value형태의 property를 갖을수 있는데 자바스크립트 함수 또한 마찬가지이다. 이 프로퍼티 중의 call, apply를 자주 볼수 있게된다. 지금까지 위에서 다뤘던 함수호출들은 execution object가 선언없이 자동으로 생성되고 소멸되었다. 즉 execution object를 프로그래머가 정할 수가 없었다.

하지만, 자바스크립트의 call&apply를 이용하면 이 execution context(this)를 명시적으로 정할 수가 있다. 이 call&apply 때문에 '생성자 빌려쓰기'라던가 '메소드 빌려쓰기' 등의 다양한 함수를 활용한 패턴을 구현할 수 있게된다. 


메소드 호출에서 사용했던 예제를 살펴보자.

{% highlight javascript linenos %}
var obj = {
  sum : function(a,b){
    this.a = a ;
    this.b = b ;
    return this.a+this.b;
  },
  isThis : function(){
    if(this === obj){
      alert('isThis is obj');
    }
  },
  nestedObj : {
    isThis: function(){
      if(this === obj){
        alert('this is obj');
      } else if(this === obj.nestedObj){
        alert('this is nested obj');
      } else if(this === window) {
        alert('this is window obj');
      }
    }
  }
} 

obj.nestedObj.isThis() ; // this is nested obj 가 출력된다. 
obj.nestedObj.isThis.call(obj) ; //this is obj 가 출력된다.
obj.nestedObj.isThis.call(window) ; //this is window obj가 출력된다.

alert(obj.nestedObj.a) ;//undefined출력
alert(obj.nestedObj.b) ;//undefined출력

obj.sum.call(obj.nestedObj,3,5) ;

alert(obj.nestedObj.a) ;//3출력
alert(obj.nestedObj.b) ;//5출력

{% endhighlight javascript %}

call을 이용하여 execution context를 명시적으로 정해준 것을 볼 수 있다. 
26번째 줄을 보면 execution object를 obj로 정해준 것을 볼 수 있다. 그러므로 obj.nestedObj.isThis.call(obj)를 실행중일때 this는 obj를 가르키며 this === obj 가 된다. 
27번째 줄을 보면 execution object를 window로 명시적으로 정해주고 obj.nestedObj.isThis를 호출하라는 의미이다.

29번째와 30번째 문장이 실행되면 undefined가 출력이 되는데, nestedObj에는 a,b 라는 key가 없기때문에 undefined가 출력이 된다.

하지만 32번째 obj.sum.call(obj.nestedObj,3,5) 가 실행이 될때를 살펴보자. 이 문장은 아래처럼 해석이 된다.
{% highlight javascript %}
sum : function(a,b){
  obj.nestedObj.a = a ;
  obj.nestedObj.b = b ;
  return obj.nestedObj.a + obj.nestedObj.b ;
}
{% endhighlight %}

그러므로 34,35번째 문장은 화면에 3,5를 출력하게 된다. 자바스크립트를 우아하게 다루기 위해서는 call&apply를 숙지하는 것은 필수 이다. 

call과 apply의 차이점은 파라미터를 다루는 방식의 차이가 있다. 
call은 일반적으로 fn.call(obj,arg1,arg2,arg3,...) 처럼 파라미터를 연속적으로 열거 하는 방식이다. 
반면에 apply는 fn.apply(obj,array) 처럼 array를 파라미터로 갖는다. ***apply같은 경우 ECMA문서를 보면 call메소드를 wrapping해서 사용하는 것을 볼 수 있다.***

> call&apply는 execution context를 명시적으로 선언한다는 점을 기억하자.
> 처음에 call,apply를 배울때 파라미터를 다루는 방식에서 누가 배열을 파라미터로 다루는지 햇갈렸는데 본인은 apply에 a를 보고 array를 연상시켰다.

자바스크립트에서 this라는 개념이 실행단계에서 정해지기 때문에 처음에는 이러한 부분들이 자바스크립트를 이해하는데 매우 혼란스럽게 한다. this가 execution context라는 점을 숙지하고 본다면 , 이러한 혼란스러운 부분은 상당부분 해소되지않을까 싶다.

***본 글은 javascript definitive guide 6th의 function invocation내용을 바탕으로 정리하였다. 원서에서는 invocation context라는 단어가 나오는데 ECMA script문서에서는 execution context라고 한다.***


