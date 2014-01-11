---
layout: post_page
title: "PHP COMPOSER 이해하기"
description: ""
category : "php"
tags: [composer]
---

PHP가 참 많은것이 변했다. 버전업이 되면서 현대의 언어들이 가지고 있는 장점들을 많이 흡수하려 했고, 객체지향 개념을 적극적으로 도입을 했다. 이 변화의 시작은 PHP 5.3이 아닐까 싶다. 그리고 FIG가 출현하면서 PHP의 룰이 명확해지고 견고해지는 느낌이다. 자바를 하다가 PHP를 접했을때는 정말 눈 버리는 느낌이었다. 코드가독성하며 변수명 앞에 붙은 '$'들은 괴상하게 보이기만 했다.  

composer를 접하고 autoload 등을 접하면서 많은 이점이 생겼다. 패키지간의 의존성관리를 자동으로 해주므로 그러한 부분에 대해서 고민할 필요가 없게됬다. 그만큼 코드를 재사용 할 수 있는 부분도 많아지게 됬음은 말할것도 없다. 

### 01. composer란 무엇인가?

***composer는 애플리케이션에서 사용하는 패키지들의 의존성을 관리해주는 툴이다.*** composer덕분에 애플리케이션의 의존성을 손쉽게 관리하고 재사용 할 수 있는 것이 많아졌다. 개발자는 패키지를 특정 애플리케이션으로부터 독립적으로 만들면 다른 애플리케이션 개발시 손쉽게 재사용 할 수 있다. 예를 들어서 회원가입 인증이라던가, 파일업로드 핸들러 부분등이 그 예가 될 것이다. 
 
### 02. Dependency Manager 가 왜 필요하나?

여기서 의존성을 관리한다는 의미에 대해서 잠깐 얘기를 하자면, 우리가 만드는 애플리케이션 들은 수많은 패키지들의 집합체이다. 또한 라이브러리를 개발시에 다른 누군가가 개발한 라이브러리를 사용하는 경우가 많다. 
> Do not reinvent the wheel! 
그러면 그 라이브러리는 다른 라이브러리와 의존 관계에 있다고 한다. 예를 들어OAuth 패키지를 개발한다고 할때, [Guzzle](http://docs.guzzlephp.org/en/latest/) 이라는 http request를 처리하는 패키지를 사용하면 OAuth 패키지는 Guzzle과 의존 관계에 있다고 한다. 그러면 OAuth 패키지를 배포할때 그 패키지를 사용하는 사람은 다시 Guzzle이라는 패키지를 받아야 한다. 이렇게 단순한 의존관계면 상관없지만, Guzzle이라는 것이 또 다른 패키지와 의존관계에 있다면? 우리는 그것을 다시 찾아서 다운받아야 한다.  

***그래서 Dependency Manager가 필요하다.***

### 03. composer 설치하기

우선 [composer 홈페이지](http://www.getcomposer.org)에 접속해보면 메인에 Dependency Manager for PHP 라고 적혀있다. 앞서 말한 것처럼 컴포저는 PHP 전용 패키지 매니지먼트다.
composer를 설치하기전에 우선 PHP 버전 확인을 해야한다. ***composer는 php의 버전이 5.3.2 이상이어야 한다.*** 

> php 버전은 console창에서 php -v 로 확인하자.

아래의 설치방법은 \*nix 기준이다. 윈도우 머신에서 설치하는 것도 홈페이지에 잘 나와있다.

composer 설치는 애플리케이션 마다 설치하는 방법이 있고, path걸린 영역에 설치해서 모든 영역에서 설치하는 방법이 있다. 아마 대부분 path설정된 영역에 설치하는 것을 선호 할 것이다. 

우선 애플리케이션 마다 설치하는 방법은 아래와 같다.  

{% highlight bash %}
cd myProject
curl -sS https://getcomposer.org/installer | php

php composer.phar  // 이러면 컴포저가 실행된다.
{% endhighlight bash %}
그러면 myProject에 composer.phar이라는 파일이 생성된다. 

그런데 매번 프로젝트 만들때마다 composer 설치하는 것이 여간 번거로운 것이 아니다. 
{% highlight bash %}
curl -sS https://getcomposer.org/installer | php
mv composer.phar /usr/local/bin/composer 
{% endhighlight bash %}

이렇게 하면 어디서나 composer 를 실행 할 수 있다.

### 04. composer.json 파일구조 이해하기

composer를 설치하면 프로젝트별로 패키지를 관리 할 수 있다. 

예를 들어서 CodeLearn이라는 프로젝트가 있다고하면 , 
CodeLearn 디렉토리 밑에 composer.json 파일을 만들고 파일안에다 필요한 내용들을 기술해주면 된다. 

이 글을 쓰는 이유는 composer.json 파일을 파악하기 위함이다. 

{% highlight bash linenos %}
{
    "repositories": [
        {
            "type": "composer",
            "url": "http://packages.cartalyst.com"
        }
    ],
        "require": {
            "cartalyst/sentry-social": "3.0.*"
            
        },
        "require-dev" : {
            "mockery/mockery": "dev-master"
        },
        "autoload": {
            "classmap": [
                "app/commands"
                ],
            "psr-0" : {
                "Member" : "app/components/",
            }
        },
        "scripts": {
            "pre-install-cmd": [
                "php artisan --env=local clear-compiled"
                ],
            "pre-update-cmd": [
                "php artisan --env=local  clear-compiled"
                ],
            "post-install-cmd": [
                "php artisan --env=local optimize"
                ],
            "post-update-cmd": [
                "php artisan --env=local optimize"
                ]
        },
        "config": {
            "preferred-install": "dist"
        },
        "minimum-stability": "dev"
}
{% endhighlight bash %}

위에는 laravel4 로 진행하고 있는 프로젝트의 composer.json 파일이다. 이거보다 더 많은 패키지들을 담고 있지만, 기본적인 내용을 전달하고자 많은 부분을 제거하였다. 

"repositories"
기본적으로 composer는 packagist.com 에서 패키지들을 검색한다. 하지만 패키지저장소는 추가 할 수 있다. 파일을 보면 repositories 부분이 있는데 저기에 저장소들을 추가 할 수 있다. packagist는 모두 공개용이므로 사내용으로 위한 패키지 저장소를 만들고 그 부분을 추가하면 composer에서는 추가된 repository도 탐색하게 된다. 더 자세한 내용은 satis 참고. 

"require"

"require-dev"

"autoload"

"scripts" 




