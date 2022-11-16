---
title: ""
excerpt: ""

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, Node]

toc: true
toc_sticky: true

date: 2022-11-15
last_modified_at: 2022-11-15
---

# Node.js

## 1. 개요

> 애플리케이션을 개발할 수 있는 자비스크립트 환경

- 원래 자바스크립트는 브라우저 내에서 동적인 작업을 처리하기 위한 언어

  - 실 코드는 C++ 구성  
    `번역 과정 상 시간 소요`

### 장점

- 자바스크립트 엔진을 사용하기 때문에 접근이 쉬움  
  짧은 Learning Curve
- 비동기 방식으로 리소스 사용량이 적음
- 다양한 라이브러리 제공

### 단점

- Native 언어로 만든 서버 환경보다는 느릴 수 있음
- 짧은 시간에 대량의 클라이언트 요청을 대응하는 웹 앱 개발에 **`적합`**
- 대량의 데이터 조회하고 긴 처리 시간을 요구하는 작업에 **`부적합`**
  - 대용량 연산 작업은 AWS의 Lambda 나 Google Cloud Functions 활용

### 웹 서버 이외의 노드

- SPA(Single Page Application)  
  Angular, React, Vue 등

- 모바일 앱 프레임워크  
  React Native
- 데스크톱 애플리케이션  
  Electron(Atom, slack, VScode, 블록 체인 앱)

### 외부 라이브러리 활용

- NPM 프로그램 사용
  - 기능을 확장한 수많은 모듈 쉽게 다운로드, 설치 가능
  - yarn 사용 빈도 증가

## 2. 설치

### 버전

- LTS  
  안정화된 버전으로 짝수
- Current  
  현재 개발 중인 버전

### 3.설치 확인

- node -v
- npm -v

### 4. IDE 설치

## 3. Node 프로젝트 만들기

### 빈 디렉토리에서 npm init 이라는 명령어로 생성 후 옵션 설정

- 옵션 중에서 package 이름과 디렉토리 이름이 같으면 배포 오류

### 옵션 설정

- package name  
  패키지를 배포할 떄 사용할 이름으로 디렉토리 이름과 달라야 함
- version  
  버전
- description  
  앱에 대한 설명
- **entry point**  
  시작하는 파일 이름
  - 앱의 출발점으로 index.js 나 App.js 사용
- test command  
  앱을 테스트 할 때 사용할 명령어
- git repository  
  git 과 연동할 때 사용할 URL
- keywords  
  패키지가 배포된 경우 사용할 검색어
- author  
  제작자
- license  
  ISC 나 MIT 를 사용하는데 오픈 소스라는 의미

### package.json 파일 생성 유무

### 프로젝트 실행

- npm start 하게 되면 entry point 로 지정한 파일이 실행
- node 파일명 을 하게되면 파일 실행
