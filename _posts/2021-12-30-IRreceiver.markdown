---
title: 적외선 수신기(IRreceiver)
date: 2021-12-29
categories: [Arduino]
layout: post
toc: true
tags: [Arduino, IoT, IRremote]     # TAG names should always be lowercase
---
- - -
아두이노를 이용하여 전등 스위치를 끄고 켤 수 있는 장치를 만들기로 했다.         

> 개발환경 : m1 mac    
> IDE : vscode의 PLATFORMIO    

# 1. #include &#60;IRremote.h	&#62;   
라이브러리를 추가하는데에는 두 가지 방법이 있다.   
## 1-1. cli에서 명령어 입력
PIO의 내장된 terminal에 다음과 같은 명령어를 입력한다.   
```shell
pio lib install "z3t0/IRremote@^3.5.1"
```   
그리고 workspace를 업데이트하기 위해 새 창을 띄워 해당 프로젝트를 다시 연다.   
main.cpp 에서 include 한다.   
## 1-2. 직접 lib 폴더에 추가   
원하는 라이브러리를 다운받아 프로젝트의 라이브러리에 추가 한다.   

# 2. 회로 구성
> 준비물 : 아두이노uno, 브레드보드, Arduino IR Sensor   
   
Arduino IR Sensor pinmap (출처: 에듀이노)   
<img src="/assets/img/IRremote/receiver_pinmap.png" width="20%" height="20%" alt="receiver_pinmap">    
Circuit design     
<img src="/assets/img/IRremote/receiver_circuit.png" width="100%" height="100%" alt="receiver_circuit">   
<img src="/assets/img/IRremote/receiver.jpeg" width="80%" height="80%" alt="receiver">   
   
# 3. 코드
```c++
//receiver
#include <Arduino.h>
#include <IRremote.h>

int recvPin = 2;
IRrecv irrecv;
IRData* irdata;

void setup() {
  irrecv.begin(recvPin, true);
  Serial.begin(9600);
}

void manufacturer(IRData* irdata){
  switch (irdata->protocol){
  case SAMSUNG: Serial.print("SAMSUNG"); break;
  case LG: Serial.print("LG     "); break;
  default: Serial.print("DEFAULT"); break;
  }
}

void command(IRData* irdata){
  manufacturer(irdata);
  Serial.print('\t');
  Serial.print("\t address: ");
  Serial.print(irdata->address, HEX);
  Serial.print("\t command: ");
  Serial.print(irdata->command, HEX);
  Serial.print("\t extra: ");
  Serial.print(irdata->extra, HEX);
  Serial.print("\t numberOfBits: ");
  Serial.print(irdata->numberOfBits, HEX);
  Serial.print("\t flags: ");
  Serial.print(irdata->flags, HEX);
  Serial.print("\t decodedRawData: ");
  Serial.print(irdata->decodedRawData, HEX);
}

void loop() {
  if(irrecv.decode()){
   irdata=irrecv.read();
   if(irdata->flags==0x40){
     Serial.print("OverFlowed\n");
     return ;
   }
  //  manufacturer(irdata);
  //  Serial.print('\t');
   command(irdata);
   Serial.print('\n');
   irrecv.resume();
  }
  irdata=nullptr;
}
```
## 3-1. 코드 설명
### IRrecv 객체  
> 수신기의 abstract이다.      

### IRData 구조체   
> 적외선 신호의 abstract이다.   

### void manufacturer(IRData* irdata);

> 이 함수는__ IRData.protocol __ 을 조사하여 제조사를 알려준다. 제조사별로 data를 encoding하는 protocol이 다르다. 위의 코드에서는 삼성과 LG만 구별하고 나머지는 DEFAULT로 나타냈다.

# 4. 실행   
만든 IR receiver를 이용하여 집에있던 리모컨들의 신호를 분석해보았다. (출력되는 숫자들은 전부 16진수)
## 4-1. Btv 리모컨
<img src="/assets/img/IRremote/Btv.jpg" width="90%" height="90%" alt="Btv">

## 4-2. WHISEN 리모컨
<img src="/assets/img/IRremote/WHISEN.jpeg" width="90%" height="90%" alt="WHISEN">

