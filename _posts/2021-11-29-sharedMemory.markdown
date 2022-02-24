---
title: shared memory
date: 2021-11-29 21:57
categories: [system programming, 개념정리]
layout: post
toc: true
tags: [system V IPC, shared memory]     # TAG names should always be lowercase
---
- - -

## Inter-Process Communication (IPC)
ipc란 프로세스 사이의 통신을 의미한다.  
ipc에는 **pipe, fifo, message queue, shared memory, semaphore** 가 있다.   
이 중에서 message queue, shared memory, semaphore 를 묶어서 **systme V ipc** 라고도 한다.

## process & memory management
OS는 **virtual memory** 라는 개념을 통해 하나의 프로세스가 무한대의 memory를 사용하는 것처럼 느끼도록 해준다.    
사용자는 os에 의해 제공되는 virtual memory 에만 접근할 수 있으며 실제 프로세스가 저장되어있는 **physical address** 에는 절대 접근할 수 없다.	
또한 서로 다른 프로세스는 절대 서로의 memory 공간에 접근할 수 없다. 
   
## shared memory
특정 프로세스들이 **RAM을 공유하는 방식**이다. 공유되는 구역을 **segment**라고 한다. 

- - -
## How to use it

### 1. IPC KEY
```c
#include <sys/ipc.h>
#include <sys/types.h>

key_t ftok(const char* path, int proj_id);
```
system V ipc 에서 사용할 key를 생성한다.   
path: key를 생성할 경로 또는 이름   
proj_id: 같은 경로 내에서 key를 구별하기 위한 값(0~255 사이의 값을 사용한다)    
return value: 성공하면 key, 실패하면 -1
   
example: 
`key_t key=ftok("abc", 'A');`

### 2. creating shared memory
```c
#include <sys/sem.h>
#include <sys/types.h>

int shmget(key_t key, size_t size, int shmflg);
```
segment 생성
key: **시스템 전체**에서 유효하다. segment를 다른 segment와 구별해준다.   
size: segment의 크기를 결정한다.   
shmflg: IPC_CREAT, IPC_EXCL    
retrun value: **local identifier** (**file descriptor**와 비슷한 개념이다)    
> 🚨 전체 system에서 local id가 같은 segment는 존재할 수 있지만 key value가 같은 segment는 존재할 수 없다.
   
example: `int shmid=shmget(key, 512, 0600|IPC_CREAT);`

### 3. attach
생성된 segment를 프로세스에 연결한다.   
```c
#include <sys/shm.h>
#include <sys/types.h>

void* shmat(int shmid, const void* shmaddr, int shmflg);
```
shmid: segment id   
shmaddr: segment를 연결할 주소. 보통 NULL(kernel이 알아서 결정한다)   
shmflg: 보통 0   
return value: 성공하면 연결된 segment의 시작 주소, 실패하면 -1
   
example: `int* shmaddr=shmat(shmid, NULL, 0);`

### 4. detatch
연결되었던 segment를 프로세스로부터 분리한다.
```c
#include <sys/shm.h>
#include <sys/types.h>

int shmdt(const void* shmaddr);
```
shmaddr: segment주소
return value: 성공하면 0, 실패하면 -1
> 🚨 detatch한다고 segment가 **삭제되는 것이 아니다**

### 5. remove
dynamic variable, open()된 file들은 프로세스가 종료되면 OS에 의해 자동으로 청소된다.    
하지만 **ipc resource는 자동적으로 지워지지 않는다.**
#### 5-1. 터미널에서 제거
```shell
ipcrm -m [shmid]
```
#### 5-2. program 내부에서 삭제
```c
#include <sys/mhm.h>

int shmctl(int shmid, int cmd, struct shm_ds* buf);
```
cmd: IPC_RMID, IPC_SET, IPC_STAT    
buf: 보통 NULL   

- - -
