---
title: RFreceiver
date: 2021-12-31
categories: [Arduino]
layout: post
toc: true
tags: [Arduino, IoT, RF]
---
- - -
RF를 이용하여 통신하는 receiver

> 개발환경 : m1 mac    
> IDE : vscode의 PLATFORMIO    

# 1. 라이브러리 추가
```shell
pio lib install "nrf24/RF24"
pio lib install "arduino-libraries/Servo@^1.1.8"
```
```c++
#include <SPI.h>
#include <RF24.h>
#include <nRF24L01.h>
#include <Servo.h>
```
# 2.회로 구성
## 🚨SPI 통신🚨
~~이것때문에 계속 안되서 3일을 헤맸다... 보드에 문제가 있는줄 알고 새로 주문했는데^^~~    
SPI 통신을 하는 모듈은 사용하는 핀이 정해져있다. SPI 통신에 대해서는 다른 포스팅에서 다루도록 하자. 결론만 설명하자면    

|모듈|아두이노|
|----|--------|
|MOSI|D11     |
|MISO|D12     |
|SCK |D13     |
|SS  |D10     |

이렇게 연결되어야 한다.

<img src="/assets/img/RFremote/RFpinmap.jpeg" width="60%" height="60%">

<img src="/assets/img/RFremote/RFreceiver.JPG" width="60%" height="60%">

# 3. 코드    
```c++
//Receiver
#include <Arduino.h>
#include <SPI.h>
#include <RF24.h>
#include <nRF24L01.h>

RF24 radio(9, 10);
int led_pin=3;
const byte address[6]="00001"; //Address value must be same in sender and receiver

void setup() {
  Serial.begin(9600);
  radio.begin();
  //RF
  radio.openReadingPipe(0, address);
  radio.setPALevel(RF24_PA_MIN);
  radio.startListening(); //Set this module as receiver.
}

void loop() {
  if(radio.available()){
    char text[32]="empty";
    Serial.print("Before: ");
    Serial.println(text);
    radio.read(text, sizeof(text));
    Serial.print("After: ");
    Serial.println(text);
    delay(1000);
  }
  else{
    Serial.println("nothing received");
    delay(1000);
  }
}
```
  
## 3-1. 코드 설명   
IPC 중 `FIFO` 와 사용방법이 유사하다.   
`void openReadingPipe(const uint8_t* address);`
> reading을 위한 pipe를 1개 open한다. data를 수신받으려면 sender와 receiver의 pipe address가 같아야 한다.   

# 3. 결과
<img src="/assets/img/RFremote/RFresult2.png" width="80%" height="80%" alt="RFresult">

