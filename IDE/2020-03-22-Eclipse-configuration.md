---
layout: post
title: "Eclipse 환경설정"
date: 2020-03-22 17:45:00 +0900
categories: [eclipse, sts]
---



## Eclipse 환경설정

- ### 인코딩방식 확인 및 설정

  File -> Properties -> Text file encoding: Eclipse 기본은 MS949로 되어있으나,
  UTF-8 방식이 일반적이므로 실제 코딩할 때는 UTF-8 방식으로 바꿔줄 것
  안그러면 UTF-8으로 입력이 되지 않기 때문에 MS-949방식으로 입력된 문자가 들어갈 것이다.



- ### 다른 사람의 jar파일을 라이브러리로 사용하는 방법

  jar파일을 자신의 패키지의 source 파일 넣는 곳에 추가
  Project 폴더 우클릭 -> 맨 아래 Properties -> 창의 왼쪽 Java Build Path
  -> 창의 위쪽 Libraries 누르고, -> 오른쪽의 Add jars 클릭 후, jar파일을 추가하고 적용

   

- ### 다른 사람에게 jar파일로 라이브러리 보내는 방법

  class 선택 후, Export 클릭 -> 창에서 Java폴더 내의 JAR file or Runnable JAR file 클릭
  -> 적당한 옵션 설정 후, 원하는 위치로 Export
  jar: 일반적으로 참조할 수 있는 형태의 jar파일로 나옴
  Runnable Jar: JRE가 설치되어 있는 PC에서 바로 구동 가능한 파일로 나옴



- ### `Import` : `Project`를 가져오는 여러가지 방식

  1. `General` : `jar` , `File System` 등의 `File`을 `Import`하는 방식

  2. `Git` : `File` - `Import` - `Git `- `Projects from git`을 통해서 git으로 관리되는 프로젝트를 가져올 수 있다.

  3. `Gradle` : `File` - `Import` - `Existing Gradle Projects`를 통해 Gradle 프로젝트를 가져올 수 있다.

  4. `Maven` : `File` - `Import` - `Existing Maven Projects`를 통해 Maven 프로젝트를 가져올 수 있다.
