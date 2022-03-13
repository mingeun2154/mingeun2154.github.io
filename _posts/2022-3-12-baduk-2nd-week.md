---
title: baduk 프로젝트 - 2주차
date: 2022-3-12 21:34
categories: [Project, baduk]
layout: post
toc: true
tags: [Swing, baduk]     # TAG names should always be lowercase
---
- - -
3.3부터 3.12까지 진행된 내용이다.

## 새로운 기능 추가

사용자가 클릭한 위치에 **돌을 놓는 기능**을 추가하였다.      
돌을 놓는 행위를 바둑에서는 '착수' 라고 한다.    

### abstraction
아래의 사진은 component들의 호출 과정이다.   
<img src="/assets/img/baduk/abstraction.png" width="70%" height="70%" alt="abstraction">    
그림에서는 생략되었지만 마지막으로 controller가 repaint()를 실행하여 domain의 변화를 view에 반영한다.    

### underlying details
* CheckerBoardController (controller)     
	<img src="/assets/img/baduk/CheckerBoardController.png" width="80%" height="80%" alt="CheckerBoardController">     
	클릭된 위치는 크기가 interval×interval인 **정사각형 구역 단위로 인식**된다.    
	예를 들어 위치가 (46,131)인 점이 클릭되었다면3행 7열의 구역이 클릭된 것으로 인식한다.    
	이 행과 열은 CheckerBoard가 유지하고 있는 2차원 배열의 index 값이다.     
        
* CheckerBoardService (service)    
	controller는 service를 통해 domain을 조작한다. **service가 호출되는 코드들을 통해 게임의 로직이 어떻게 구현되는 지를 파악**할 수 있다.      
	    
* CheckerBoar (domain)
	바둑판의 점들을 나타내는 2차원 배열 원소의 data가 변하게 된다.   
	    
### representation     

<img src="/assets/img/baduk/representation.png" width="80%" height="80%" alt="representation">       
      
grid[i][j]는 바둑판의 i번째 선과 j번째 선의 교점을 의미한다.    
index와 선의 번호를 일치시키기 위해 20x20 배열을 선언하였다.
