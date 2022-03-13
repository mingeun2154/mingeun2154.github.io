---
title: baduk 프로젝트 - 1주차
date: 2022-3-3 00:04
categories: [Project, baduk]
layout: post
toc: true
tags: [Swing, baduk]     # TAG names should always be lowercase
---
- - -

바둑판 프로그램을 시작한지 1주일이 지났다.     
그동안 진행된 내용들이다.   

## component
[design 단계](/posts/baduk-design/#components)와 비교해서 다음과 같이 바뀌었다.  
* __View__ (추가됨) : Swing Container들의 집합이다.   
**xxxScreen 클래스들은 'xxx화면'을 나타낸다.** 예를 들어 StartScreen은 시작화면을,   
NewGameScreen은 시작화면에서 '새 게임' 버튼을 눌렀을때 나오는 화면이다.   
MainFrame 클래스는 JFrame 객체이다. 최상위 컨테이너로 프로그램이 실행되는 창(window)를 의미한다.     
* __content__ (추가됨) : 각각의 화면을 구성하는 Swing Component들이다.     
이 객체들은 view 객체들이 생성될 때 그 객체들에 add된다.   
* __TestFrame__ (추가됨) : **테스트를 위한 JFrame이다.** 하나의 Screen만을 띄워보고 싶을 때 이 frame에 추가해서 화면에 띄운다.     
새로운 화면 GUI를디자인 할때 이 클래스에 넣어서 먼저 테스트 한 뒤에 main 코드에 적용시키기 위해 만들었다.

## StartScreen (시작 화면)
MainFrame(JFrame) <- StartScreen(JPanel) <- NewGameButton/LoadGameButton(JButton)
<img src="/assets/img/baduk/startScreen.png" width="60%" height="60%" alt="시작 화면">

## CheckerBoard (바둑판)
TestFrame(JFrame) <- NewGameScreen(JPanel) <- CheckerBoard(JPanel)
<img src="/assets/img/baduk/checkerBoard_explain.png" width="100%" height="100%" alt="explain">      
       

<img src="/assets/img/baduk/checkerBoard_mockup.png" width="60%" height="60%" alt="test">

## 고민
1. 현재 StartScreenController는 두 버튼에 대한 클릭을 감지한다. 클릭된 버튼에 따라 그에 맞는 화면으로 전환하는 기능을 구현할 예정이다.   
그런데 이와 같은 **단순 화면 전환도  controller를 거쳐서 진행되어야하는지**가 의문이다. 딱히 model의 data를 바꾸는 일이 아니기 때문이다. 그렇다고 view에서만 처리하기에는 사용자와 상호작용하는 것은 controller의 역할이다.     
2. controller가 클릭을 감지하고 화면을 바꾸기 위해서는 controller에서 view에게 접근이 가능해야 한다.    
구체적으로 __StartScreenController가 현재 띄워진 StartScreen JPanel 또는 StartScreen이 붙어있는 전체 MainFrame 객체를 가지고 있어야 한다.__     
프레임워크의 도움 없이 이걸 구현하는 방법은 __MainFrame을 싱글톤 객체로 만드는 방법__ 이 유일한 것 같다.       
그래야 클래스 외부에서 공통된 유일한 객체에 접근할 수 있기 때문이다.       
