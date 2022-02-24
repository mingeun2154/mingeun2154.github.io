---
title: 멀티부팅 환경에서 블루투스 키보드 연결하기
date: 2022-2-15 22:34
categories: [개발환경]
layout: post
toc: true
tags: [bluetooth, keyboard]     # TAG names should always be lowercase
---
- - -

듀얼부팅 환경에서 블루투스 키보드를 연결하는 방법은 다음과 같다.

1. Ubuntu에서 먼저 키보드와 PC를 연결

2. Windows에서 키보드와 PC를 연결    
2-1. [PSTools][https://download.sysinternals.com/files/PSTools.zip]를 다운    
2-2. cmd를 관리자 권한으로 실행한 뒤 PSTools 가 설치된 경로로 이동    
2-3. `cd C:\Users\유저이름\Downloads\PSTools` 로 이동(한글인 부분은 사람마다 다르다)   
2-4. `psexec -s -i regedit` 입력   
2-5. Computer->HKEY_LOCAL_MACHINE->SYSTEM->CurrentControlSet->Services->BTHPORT->Parameters->Keys 로 들어간다    
2-6. 레지스트리 편집기의 왼쪽화면에 연결된 기기의 맥주소(이름)와 페어링 키(데이터) 가 나온다
2-7. 내가 연결한 키보드의 맥주소에 해당하는 페어링 키 값을 메모한다

3. Ubuntu로 로그인
3-1. 터미널을 열고 `sudo su`로 사용자를 바꾼다     
3-2. `cd /var/lib/bluetooth/블루투스 어댑터 맥주소/키보드 맥 주소`    
3-3. `vim info`     
3-4. [LinkKey]의 `Key` 변수에 아까 메모한 값을 대입한다     
`Key=키보드의 페어링 키`

위 과정은 윈도우와 키보드, 우분투와 키보드 사이의 페어링 키 값을 통일시키는 과정이다.

<img src="/assets/img/bluetooth_keyboard/keychronK3.jpeg" width="50%" height="50%" alt="keychronK3">
