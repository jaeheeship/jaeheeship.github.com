---
layout: post_page
title: "DOCKER 정리 - 01 [ Single Respoinsibility]"
description: ""
category: "php"
tags: [namespace]
--- 

DOCKER엔진은 크게 두 부분으로 구성되어 있다. 모든 컨테이너를 관리하는 서버 프로세스 데몬 , 그리고 프로세스 데몬을 원격조정하는 클라이언트 이다. 

가장 손쉽게 시작하는 방법은 이미 만들어 놓은 컨테이너 이미지를 사용 하는 것이다. 컨테이너 이미지들은 Docker Hub Registry에 있다. Docker Hub에 존재하는 이미지들은 클라이언트 명령어로 찾을 수 있다. 

컨테이너 이미지들은 docker pull 명령어로 받을 수 있다. git 같은 VCS에 익숙하다면 익숙 할 것이다.
