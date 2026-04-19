---
title: "&#91;Vert.x&#93; Vert.x 를 시작해보자"
layout: profession-post
categories:
  - Programming
tags:
  - VertX
  - BackEnd
toc: true
toc_sticky: true
type: profession
---

![Vertx 로고][1]

이제 지루한 이론 설명을 치우고, 실전으로 돌입할 차례이다!

이번 포스트에서는 Vert.x 초기 세팅에 대해서 다뤄볼 것이다.

시작하기에 앞서, 본 포스트에서 사용하는 개발환경에 대해서 적어놓겠다.

```java
VScode
Maven

JDK 1.8
Vert.x 3.9.2
```

경험상 위와 다르면 소스코드를 그대로 갖다 붙여넣는다고 해도 버그가 발생할 수 있다.

Vert.x 를 시작하는 방법은 여러가지가 있는데, 개인적으로 권장하는 방법을 소개하고자 한다.

## 1. Vert.x 홈페이지 이용하기

![Vertx Homage][3]

[Vert.x 홈페이지][2]에 들어가서 Starter 메뉴에 들어간다.

![Vertx Starter][5]

위와 같이 설정하고, Generate Project를 누르자. 그러면 뭐가 다운로드 된다.

![Vertx Folder][6]

압축을 풀어보면 이같이 구성되어있다.

각 파일의 역할은 다음과 같다

- mvnw : maven을 설치하지 않고도 쓸 수 있게 해줌
- pom.xml : maven 세팅 파일
- editorconfig : editing 설정 파일

## 2. 프로젝트 생성

이걸 그대로 VScode 에서 열어보자.

**<span style="color:red">VScode 자바 세팅이 되어있어야 한다!!</span>**

![vscode auto][7]

자바 세팅이 되어있으면 우측 하단에 이렇게 auto import 가 뜬다. 클릭해주자.

![vscode project][8]

조금 기다려주면 이렇게 Java 프로젝트가 생성되어있다.

## 3. 테스트

제대로 생성되었는지 확인해보자.

![vertx test run][9]

TestMainVerticle 에 들어가서 Run Test를 눌러보자.

![test result][10]

이렇게 Http Server 가 열리면 성공이다!

이제 Vert.x 를 시작할 수 있다!

[1]: https://upload.wikimedia.org/wikipedia/commons/thumb/c/c4/Vert.x_Logo.svg/1200px-Vert.x_Logo.svg.png
[2]: https://vertx.io/
[3]: /assets/vertx/start_vertx/vertx_homepage.PNG
[5]: /assets/vertx/start_vertx/vertx_starter.PNG
[6]: /assets/vertx/start_vertx/vertx_folder.PNG
[7]: /assets/vertx/start_vertx/vscode_auto.PNG
[8]: /assets/vertx/start_vertx/vscode_project.PNG
[9]: /assets/vertx/start_vertx/vertx_test_run.PNG
[10]: /assets/vertx/start_vertx/test_result.PNG