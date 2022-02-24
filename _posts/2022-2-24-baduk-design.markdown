---
title: 바둑 연습 프로그램-설계
date: 2022-2-24 15:57
categories: [Project]
layout: post
toc: true
tags: [JAVA, 바둑]     # TAG names should always be lowercase
---
- - -

Java의  Swing 라이브러리를 사용해 바둑을 연습할 수 있는 프로그램이다.     
    
## 핵심 기능
* 착수
* 차례 알려주기
* 시간 제한
* 뒤로가기, 앞으로
* 전체 내용 저장 및 불러오기

## Service Flow
				     
<img src="/assets/img/baduk/service_flow.png" width="100%" height="100%" alt="service_flow">

## Components Structurs
[**MVC 패턴**](/posts/MVC/ "MVC 패턴 알아보기")을 적용하여 패키지를 다음 components로 나누었다.
* controller : 사용자로부터 받은 입력을 처리한다.
* service : domain을 조작하여 game logic 을 구현한다. 
* repository : domain의 상태를 저장소에 저장한다. 저장소는 사용자의 pc를 사용할 예정이다. interface로 만들어 추후에 저장소를 바꿀수 있도록 한다.
    
* domain : 게임을 구성하는 요소들이다. data로 이루어지며 **사용자가 조작하는 대상**이다.(Model에 해당.)
    
* renderer : Model을 화면에 그린다.(View에 해당한다.)
     
## Dependencies
각 component의 의존관계는 다음과 같다.   
service, renderer, repository는 domain을 의존관계(dependency)로 가진다.
      
<img src="/assets/img/baduk/dependencies.png" width="80%" height="80%" alt="dependencies">
      
## Components 구현
