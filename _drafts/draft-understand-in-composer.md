---
layout: post_page
title: "PHP COMPOSER 이해하기"
description: ""
category : "php"
tags: [composer]
---

PHP가 참 많은것이 변했다. 버전업이 되면서 현대의 언어들이 가지고 있는 장점들을 많이 흡수하려 했고, 객체지향 개념을 적극적으로 도입을 했다. 이 변화의 시작은 PHP 5.3이 아닐까 싶다. 그리고 FIG가 출현하면서 PHP의 룰이 명확해지고 견고해지는 느낌이다. 자바를 하다가 PHP를 접했을때는 정말 눈 버리는 느낌이었다. 코드가독성하며 변수명 앞에 붙은 '$'들은 괴상하게 보이기만 했다.  

composer를 접하고 autoload 등을 접하면서 많은 이점이 생겼다. 패키지간의 의존성관리를 자동으로 해주므로 그러한 부분에 대해서 고민할 필요가 없게됬다. 그만큼 코드를 재사용 할 수 있는 부분도 많아지게 됬음은 말할것도 없다. 

composer 홈페이지에 젒속하면서 메인에 Dependency Manager for PHP 라고 적혀있다. 말그대로 composer는 PHP를 위한 패키지 매니지먼트 이다. 

설치방법은 간단하므로 링크로 대체한다. 

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




