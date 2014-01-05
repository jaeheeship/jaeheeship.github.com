---
layout: post_page
title: "vim 설정파일 알아보기"
description: ""
category: "console"
tags: [vim]
---
웹 개발을 하면서 여러가지 에디터를 사용해보고, vim이 아닌 다른 에디터를 익숙해지려고 노력해봤는데 다시 원점은 VIM . 물론 고수들 만큼 vim을 잘 다루는 것은 아니지만 작업할때 vim이 참 편한건 사실이다.다른 에디터를 사용해도 그 에디터를 위한 vim플러그인을 찾을 정도이다. 최근에는 frontend 작업이 많아서 [bracket에디터](http://brackets.io/)를 잠깐 써봤는데 매력적이긴 한데 화살표로 커서이동하는 것이 여간 불편한 것이 아니다. 결국엔 다시 VIM! 
vim설정파일은 필요할때 마다 하나씩 찾아가면서 배운것이라 정확히 알지 못했다. 초심으로 돌아가 vim을 정확히 파악하고자 정리를 하기 시작한다. 

### .vimrc 

.vimrc 파일은 홈디렉토리 밑에 있다. \*nix 시스템 기준
{% highlight bash %}
vi ~/.vimrc //만약에 없다면 만들고 설정하면 된다.
{% endhighlight bash %}

오늘은 mapping 에 대해서 간단히 정리해 본다. 
vim의 매력은 키보드로 모든 것을 컨트롤 한다는 것인데, 그러한 장점을 살려주는 것이 키매핑이 아닐까? 설정파일만 보면 어떤 역할일지 감이 오지만, 어설프게 알면 아는것만 못하다고 했다. 이번기회에 매핑에 대해서 정리해 본다.

###  map vs noremap
map,noremap 둘다 특정키를 매핑하는 역할을 한다. 간단히 얘기하면 recursive vs non-recursive 이다. 
에를 들어서

{% highlight bash %}
:map j gg
:map Q j
{% endhighlight bash %}

를 하면 j를 누르면 gg를 누른 것과 동일한 효과를 얻게 된다. 그렇다면 만약에 대문자 Q를 누른다면? j로 매핑한다음에 다시 j가 gg를 가리키므로 결과적으로 Q는 gg를 수행하게 된다. . 그래서 map을 'recursive'하다라고 얘기한다. 매핑되는 키의 또다른 매핑되는 키를 찾아가게 된다.<br/> 
Q -> j -> gg 이렇게 됩니다.

{% highlight bash %}
:map j gg
:map Q j
:map gg Q
{% endhighlight bash %}
궁금해서 이렇게 무한 재귀 돌리면 어떻게 되나 봤는데 , 오류납니다. <br/>
Q->j->gg->Q 이런식으로 흘러가는데, 처음 누른키와 중간의 맵핑되는 키가 같게되면 예외가 발생시키게 한거 같아요.


하지만 noremap 은 다릅니다. 
noremap은 non-recursive map 이라고 생각하시면 됩니다. 
{% highlight bash %}
:map j gg
:noremap Q j
{% endhighlight bash %}
Q를 눌르면 j를 누르는 것과 동일한 효과 입니다. 
예문은 [스택오버플로우](http://stackoverflow.com/questions/3776117/vim-what-is-the-difference-between-the-remap-noremap-nnoremap-and-vnoremap-ma) 에서 발췌 했습니다.
설정파일 보면 대략 어떻게 작동될거라 예상은 되었지만, 이런 미세한 차이가 있는건 설정파일 정리하면서 알게됬습니다.  

### &lt;leader &gt;
&lt;leader &gt; 는 기본적으로 '\\'를 가리키게 됩니다. 예를 들어서 

{% highlight bash %}
:map <leader>A g
{% endhighlight bash %}
이러면 '\\A'를 누르면 g를 가리키게 됩니다.

{% highlight bash %}
:let mapleader="," 
:map <leader>A g
{% endhighlight bash %} 
그런데 leader매핑되는 키를 바꿀수가 있는데요 위에 처럼 작성하면 &lt;leader&gt;는 ',' 가 되고 ',A'를 누르면 g를 가리키게 됩니다.
공유되는 .vimrc 파일보면 자주 볼수 있는 부분이 mapleader="," 입니다. 아무래도 '\'같은 경우엔 손을 더 많이 움직여야해서 ','로 대체하는거 같은데 그렇게 설정하니까 편하네요. 

키맵핑에 관한 자세한 팁들은 
[vim.wikia.com/wiki](http://vim.wikia.com/wiki/Mapping_keys_in_Vim_-_Tutorial_%28Part_1%29) 여기서 더 많이 볼 수 있습니다. 

|명령어            | 설명|
|------------------|-------------|
|nmap              | display normal mode maps  |
|imap              | display insert mode maps  |
|vmap              | display visual and select mode maps  |
|smap              | display select mode maps  |
|xmap              | display visual mode maps  |
|cmap              | display command-line mode maps |
|omap              | display operator pending mode maps |

<br/>
거기서 퍼왔는데 위의 명령어를 보시면 대략 감이 오실 겁니다. 그러면 이제 vimrc를 보는데 훨씬 수월하게 읽히실 겁니다. <br/>

또한,vim은 일관성있는 명령체계 덕분에 몇개 명령어만 알면 명령어를 유추해볼수 있는데요. 
{% highlight bash %}
:nnoremap g gg
{% endhighlight bash %}
이렇게 매핑한다면 normal mode일때 g를 누르면 gg로 non-recursive 매핑을 하게됩니다. 

마지막으로 vim에서 keymapping 시에 enter,esc,F1 등의 키를 매핑을 할 수도 있다. 

| vim mapping key             | 대응되는 key
|-----------------------------|---------------|
|&lt;BS&gt;                   |Backspace      |
|&lt;Tab&gt;                  |Tab            |
|&lt;CR&gt;                   |Enter          |
|&lt;Enter&gt;                |Enter          |
|&lt;Return&gt;               |Enter          |
|&lt;Esc&gt;                  |Escape         |
|&lt;Space&gt;                |Space          |
|&lt;Up&gt;                   |Up arrow       |
|&lt;Down&gt;                 |Down arrow     |
|&lt;Left&gt;                 |Left arrow     |
|&lt;Right&gt;                |Right arrow    |
|&lt;F1&gt; - &lt;F12&gt;     |Function keys 1 to 12       |
|#1, #2..#9,#0                |Function keys F1 to F9, F10 |
|&lt;Insert&gt;               |Insert                      |
|&lt;Del&gt;                  |Delete                      |
|&lt;Home&gt;                 |Home                        |
|&lt;End&gt;                  |End                         |
|&lt;PageUp&gt;               |Page-Up                     |
|&lt;PageDown&gt;             |Page-Down                   |

예를 들어서 
{% highlight bash %}
:imap ,<Space> <Space><Space><Space><Space>
:nmap ,t :!phpunit tests
{% endhighlight bash %}

이렇게 작성한다면 insert mode일 경우 ,+스페이스바 를 입력하면 공백이 4개가 입력됩니다.
또한 normal mode에서 ,t를 누르면 :!phpunit tests 가 입력이 되서 phpunit이 실행됩니다. 

