---
layout: post_page
title: "Mockery 경험담 (01)"
description: ""
category : "php"
tags: [mockery]
---

PHP도 TDD를 많이 도입하는 추세이다. ruby나 python 진영에 비하면 아직이지만 배포되는 패키지에 TESTCASE들이 있는것을 종종보는데 PHP도 TDD 가 익숙해지는 거 같다. 하지만 정작 나에겐 TDD는 아직 먼나라 얘기이다. 지금 개인적으로 진행하는 프로젝트는 TDD형태로 개발을 진행해 나가고 있다. 하지만 오늘 계속된 실수속에서 거의 진도를 나가지 못했다. 물론 그속에서도 배운건 좀 있다. 

오늘 코딩한 내용의 일부를 발췌하면 아래와 같다. 

{% highlight php linenos %}
<?php  
use \Mockery as m;

use Leanning\LearningManager ; 
use Learning\LearningRepository ; 
use Learning\LearningRepositoryInterface ; 

class LearningTest extends PHPUnit_Framework_TestCase
{
    protected $learningManager ;
    protected $learningRepository ; 

    public function setUp()
    {
        $this->learningManager = new LearningManager(
            $this->learningRepository = m::mock('Learning\LearningRepositoryInterface') ;
        ); 
    }

    public function tearDown()
    {
        m::close(); //반드시 있어야함.
    }

    public function testLearningRegister()
    { 
        $data = array(
            'subject' => 'math' 
        ); 
        $this->learningRepository->shouldReceive('register')->once()->andReturn($data) ; 
        $this->learningManager->register($data) ; 
    }

}
{% endhighlight php %}

### 실수 1 : autoload 깜빡하기 
가장 처음에 한 실수이다. 처음에는 class를 찾을수 없다는 메시지가 나와서 당황스러웠다. 네임스페이스도 정확히 맞췄고, 클래스네임에 오타 없음도 분명히 확인했는데 여전히 클래스를 찾을수 없다고 했다. 그런데 생각해보니 phpunit은 autoload를 해주지는 않는다. 클래스가 구현된 파일들을 include 하지 않았으니 class를 찾지못하는 것은 당연하다. 지금 생각해보면 너무나 당연한 사실이지만, 기존에 autoload에 익숙해져서 클래스를 include하는 것을 깜빡했다. 

그러면 테스트 파일들 마다 필요한 class들을 include해야하나? 하지만 내가 기존에 봤던 테스트케이스 들은 그런적이 없었는데... 
검색해보니 phpunit을 실행할때 bootstrap 옵션이 있어서 옵션에 지정한 php 파일을 불러오는 것이 가능했다. 
{% highlight bash %}
phpunit --bootstrap="vendor/autoload.php" test
{% endhighlight bash %}

하지만, phpunit등의 테스팅 도구를 이용하는 것은 테스트의 자동화인데, 저렇게 명령어 뒤에다 옵션을 붙이는 것은 참 거추장스러워 보인다. 내 기억으로는 다른 패키지는 phpunit 한번이면 다 해결이 됬는데 왜 나는 그렇게 안되지? 생각해봤다. 
그 해결책은 바로 phpunit.xml 파일이다. 
{% highlight xml %}
<phpunit bootstrap="vendor/autoload.php"
        colors="true"
        convertErrorsToExceptions="true"
        convertWarningsToExceptions="true"
        convertNoticesToExceptions="true"
        stopOnFailure="true"
</phpunit>
{% endhighlight xml %}
이렇게 phpunit.xml 파일을 만들고 phpunit을 실행하면 저 환경설정 파일을 읽어들인다음 테스트를 실행한다.

### 실수 2 : Mockery 기본 셋팅하기
Mockery::close()  이 문장은 반드시 필요하다. 이부분은 조금더 공부가 필요한데 , Mockery::close()를 안하면 테스팅이 정상적으로 작동이 안된다. 예를 들면
{% highlight php %} 
<?php
public function testLearningRegister()
    { 
        $data = array(
            'subject' => 'math' 
        ); 
        $this->learningRepository->shouldReceive('register')->once()->andReturn($data) ; 
        $this->learningManager->register($data) ; 
    }
?>
{% endhighlight php %}

여기서 ***shouldReceive('register')->once() 문장은 learningRepository가 반드시 register라는 메소드를 한번은 실행해야 한다는 것이다.*** Mockery::close() 가 없으면 저 테스팅이 정상적으로 이뤄지지 않는다. register메소드가 호출이 되지 않았음에도 성공적으로 테스팅이 종료된다.  

여기까지가 하루종일 한 결과물이다. 영상 튜토리얼도 많이보고 그랬지만, 역시나 한번 손으로 만지면서 하면 또다른 느낌이다. 영상속에서는 당연하다고 느꼈던 분들이 손으로 칠때는 낯선 경험을 하는듯 하다. 마치 좋은글을 많이봤지만 막상 쓰려고 할때는 연습없이는 좋은 글이 나오지 않는것처럼 프로그래밍 언어도 마찬가지이다. 

난 그래서 ***'생각하는 프로그래머'***는 아티스트라고 생각한다.

### Mockery에 대해서 간단한 설명
Mockery는 mock객체를 테스트 할수 있도록 해주는 Tool이다. 예를 들어서 Database관련 코딩을 하고 테스팅을 한다고 할때 실제적으로 dummy데이터를 database에 집어넣는 등의 테스트를 하고 그것을 다시 삭제하고를 반복해서 테스트한다. 하지만 Mock객체를 활용하면 이러한 것들을 가상으로 작동하게끔 해서 테스트를 조금더 손쉽게 해준다. 실제적으로DB에 값을 입력하지 않고도 테스트를 행할수 있다. Mockery 는 조금더 공부하고 난뒤에 정리를 할 예정이다. 

### 현재까지 TDD를 하면서 느낀점. 
* ***코드의 품질이 자연스럽게 높아진다.*** 테스팅이 용이하려면 각 클래스간의 독립성이 강해져야 한다. 그러므로 클래스는 더 작아지고 견고해진다. 왜냐하면 쉬운 테스팅 코드를 작성하려면 Dependency Injection 도 해야하고 Interface구현하는것이 용이하다. 이러한 부분을 따르다보면 자연스럽게 코드의 설계가 좋아진다. 
* ***TDD하는것이 많은 시간이 들까? 학습비용이 있는건 사실이다. 하지만, 그 학습을 통해서 얻어지는 것이 많다.*** 나만하더라도 TDD 를 준비하면서 Dependency Injection이나 IoC , SOLID principle 등을 다시 정리하고 그랬다. 초기에는 테스팅코드 작성하는 것이 시간이 더 걸리는것은 사실이다. 하지만 프로젝트 중반쯤으로 넘어가면 그간 누적된 테스팅 코드가 프로그램의 신뢰성을 높여줄 것이고 QA하는 시간또한 많이 단축되줄 것이라 생각한다.  만약에 테스팅 코드 없으면 프로젝트 막판까지도 지금까지 구현한 코드를 다시 검토하는데 많은 시간이 걸릴 것이다. 
