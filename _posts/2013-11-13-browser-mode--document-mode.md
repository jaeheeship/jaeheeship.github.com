---
layout: post_page
title: "browser mode 와 document mode"
description: ""
category: "html"
tags: [ie]
---


웹개발을 하면서 이런 기본적인 부분에 대해서 간과하기 싶다. 몇년동안 아무 생각없이 컨트롤 C,P에 의존하며 무의식적으로 쓰던 코드중에도 눈여겨봐야 할 부분이 있다. html코드에는 docytype이라는 부분이 명시되어 있는데 이부분을 별로 중요하게 생각하지 않았지만, 요근래 들어 중요성을 깨닫고 차근차근 관련문서도 찾아보면서 정리를 했다. 

### Doctype이란 무엇인가? 
***HTML 문서 형식에 관한 버전 정보를 설정하는 역할을 하는 요소이다.*** 반드시 모든 HTML 문서의 맨 앞에 있어야 하며, 그 앞에 어떠한 문자도(공백도포함) doctype보다 먼저 나와서는 안된다. HTML구성요소는 아님을 알고 있자. doctype을 선언하므로써 현재의 html문서의 버전이 무엇인지를 정확히 명시한다. 이것을 정확히 명시하지 않으면 quirks mode(호환모드)로 해석을 하게된다.  이러면 브라우저마다 렌더링 방식이 달라진다.

### Doctype이 하는 역할은 무엇인가? 
앞서 얘기했듯이 html문서의 버전을 알려주는 역할을 한다. 브라우저에게 html5인지 html4.01 또는 xhtml1.1 등의 버전정보를 알려줌으로써 브라우저는 전송받은 html문서를 어떻게 렌더링해서 보여줄지 결정하게 된다. 만약에 doctype이 없다면 html5를 준수하는 마크업을 했다고 하더라도 브라우저는 이것을 html5문서로 해석을 못한다. doctype이 없으므로 quirks mode로 렌더링을 하게되는 불상사가 일어난다. 이것은 익스플로어 호환성 작업에서 매우 치명적임을 알고있자.

### Document 모드는 무엇인가? 

### 표준은 언제 나온것인가? 

### 그렇다면 quirks 모드는 무엇인가?

### 어떤코드가 정확히 작동할까? 
현재 html5 마크업이 대세 이므로 이렇게하면 어느 브라우저에서건 standard mode로 해석되므로 매우 쾌적한 마크업작업을 진행 할 수 있다.
{% highlight html %}
<!doctype html>
<html>
    <head>
        <meta charset="utf-8">
        <meta >
    </head>
    <body>
    </body>
</html>
{% endhighlight html %} 


흔하게 쓰는 Doctype에 대해서 얼마나 알고 있을까? 실제로 html 작업을 많이 하지는 않아서, 그 차이점에 대해서는 자세히 알지 못했는데 이번에 IE8 호환성 작업을 하면서 많이 공부하게 되었다. QUIRKS MODE , STANDARD MODE 에대해서 정리해 본다. 

참고 링크 
* [http://msdn.microsoft.com/en-us/library/cc288325(v=vs.85).aspx]
* (http://msdn.microsoft.com/en-us/library/gg699340(v=vs.85).aspx)
* (http://msdn.microsoft.com/en-us/library/gg699338(v=vs.85).aspx)
* (http://msdn.microsoft.com/en-us/library/jj676917(v=vs.85).aspx)
