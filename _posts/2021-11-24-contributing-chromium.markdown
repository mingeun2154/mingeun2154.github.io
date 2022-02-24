---
title: Contributing to Chromium
date: 2021-11-24 09:04
categories: [chromium, study]
layout: post
toc: true
tags: [chromium, commands]     # TAG names should always be lowercase
---
- - -

[더 자세한 내용은 여기로](https://chromium.googlesource.com/chromium/src/+/main/docs/contributing.md#Initial-git-setup)

## 1. creating a change
`$ git checkout -b mychange -t origin/main`   
change를 위한 브랜치 생성. origin/main을 upstream으로 설정	

## 2. commit checklist 
* rebase your repository   
`$ git pull && gclient sync -D`   
작업 안하고 rebase만 하는 main branch에서 실행

* build   
`$ autoninja -C out/Default Chrome`    

* formatting codes  
`$ git cl format`       

	[더 자세한 내용](https://chromium.googlesource.com/chromium/src/+/main/docs/commit_checklist.md)

## 3. commit your change locally in git
`$ git commit -a`     
-a : 수정되거나 삭제된 파일들을 자동으로 staging한다. 새로 생성된 파일은 직접 add해야 한다.   

## 4. upload the CL to Gerrit
`$ git cl upload`    

## 5. find file owners
`$ git cl owners`    
[cs.chromium](https://source.chromium.org/chromium)  에서 직접 OWNERS 파일을 열어 확인할 수도 있다.

## 6. go to the Gerrit
`$ git cl web`    
## Run Chromium
`$ out/Default/chrome`    
