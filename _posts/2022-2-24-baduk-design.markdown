---
title: baduk 프로젝트 - 설계
date: 2022-2-24 15:57
categories: [Project, baduk]
layout: post
toc: true
tags: [JAVA, baduk]     # TAG names should always be lowercase
---
- - -

Java의 **Swing 라이브러리**를 사용해 **바둑을 연습할 수 있는 프로그램**이다.     
    
## 핵심 기능
* 착수
* 차례 알려주기
* 시간 제한
* 뒤로가기, 앞으로
* 전체 내용 저장 및 불러오기

## Service Flow
				     
<img src="/assets/img/baduk/service_flow.png" width="100%" height="100%" alt="service_flow">

## Components
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

### domain  
**data 위주로 필드와 메소드 이름을 정의**한다. 
   
* stone : 바둑돌의 abstract이다. 착수의 대상이다.    
	- 바둑돌의 물리적인 크기(반지름), 색깔을 담고 있다.     
* board : 바둑의 abstract이다. 2차원 배열 Stone [19][19] stones_positions에 바둑돌의 유무를 저장한다. 
	* clear(), erase_oneStone() 등의 메소드를 제공한다.    
* game : 하나의 대국 전체에 대한 abstract이다. 총 걸린 시간, 놓인 돌들의 위치를 담고 있으며 저장, 불러오기 기능의 대상이다.    
* clock : 시계의 abstract. 제한시간, 진행시간을 화면에 출력할때 사용된다.
* button : 다양한 버튼들의 [**바탕**](/posts/baduk-design/#interface-vs-inheritance-)이 된다.   
#### interface vs inheritance ❓
> 여러 객체들의 공통되는 성질을 정의하기 위해 **interface**와 **상속** 중 어떤 방법을 사용해야 하는 것이 좋을까?    
> 한 번에 여려개의 객체가 필요한 상황이기 때문에 **하나의 button으로부터 상속받는게** 낫다고 생각했다. 생각해보니 굳이 interface를 구현할 필요는 없다.

### service 
 컨트롤러에 의해 호출되며 서비스들이 호출되는 과정은 곧 **게임이 실행되는 logic이 된다**. 

* Boardservice : 특정 좌표에 대해 돌의 유무를 조작한다. 
 	* setStone(), resetGame(), takeStoneBack()등의 메소드를 지원한다. 각각 착수, 처음부터 시작하기, 한수 물리기에 해당한다.
* ButtonService: IsPushed()를 통해 버튼이 눌려졌는지 확인한다. 눌러졌다면 해당 버튼의 기능을 실행한다.(Domain을 조작한다.)

### controller 
**사용자의 입력을 감지**하고 적절한 **service를 이용해 Domain을 조작**한다.
* BoardController : click을 감지하여 최종적으로 그 자리에 돌이 출력되도록 만든다.
