---
title: 'Dev Log \| 미세톡톡 개발 일지 1'
excerpt: 'README 문서 개선'
date: 2020-08-12
last_modified_at: 2020-08-15T20:33:29
toc: false

categories:
  - Development Logs

tags:
  - Development Logs
  - Dev Log
  - 미세톡톡
---

> 본 프로젝트는 휴먼스케이프와 함께한 프로젝트입니다.
>
> 휴먼스케이프에서 서비스 중인 미세톡톡이란 모바일 어플리케이션을 개선하였습니다.

**프로젝트 이름** 미세톡톡

**프로젝트 기간** 2020.07.16 - 2020.08.07

## README 문서 개선
휴먼스케이프와 기업 협업 프로젝트를 하게 되면서 가장 먼저 해야할 일은 개발환경 세팅을 끝내고 README 문서를 개선하는 작업이었습니다.

특히 README에 관해서 ***이 프로젝트를 처음 접하는 사람이 README만 보고도 프로젝트에 바로 합류할 수 있을 정도로 개선*** 하는 것이 휴먼스케이프의 요구였습니다.

### 변경 전
기존의 리드미에는 Reactotron 설정 방법, version 관리, Firebase 계정, API 모드 스위치 방법 등이 적혀 있었습니다.
React-Native를 세팅하고 앱을 실행하는 작업은 위키 문서에 적혀있고 리드미는 위키 문서의 링크만 제공했습니다.

### 개선 과정
#### 내용 재배치
이번 리드미 개선 작업에서는 트러블슈팅과 관련된 내용까지 리드미 문서 안에 다 포함했습니다.

대신 트러블슈팅은 문서의 가장 아랫부분에 배치해서 링크로 찾아가도록 하고 윗부분에는 일반적인 사용법만을 정리했습니다.

또한 리엑트 네이티브 공식문서의 설명이 워낙 잘 되어 있었습니다.
그래서 세팅, 실행, 디버깅 등에 해당하는 공식문서의 링크를 첨부했고, 한두 번 구글링을 거쳐야 하는 문제들이나, development kit 버전 정보를 덧붙이는 식으로 개선 방향을 잡았습니다.

특히 내용이 분산돼 있어서 여러 페이지를 동시에 이동하며 봐야 했던 기존 문제를 개선해 실제 세팅과 실행을 하면서 거치는 단계대로 리드미에 해당 내용이 등장하도록 순서에 신경을 썼습니다.

#### Ubuntu 전용 설명 추가
이렇게 react-native를 사용해서 하이브리드 앱을 개발할 때는 macOS를 사용하는 것이 일반적이지만, 저는 아직 맥북을 갖추지 못해서 ubuntu 환경에서 진행했습니다.
그러다 보니, 무언가를 설치할 때나 path 설정을 할 때 커맨드가 미묘하게 리드미 문서에 적혀있는 것과 달랐고 이 부분에 대해서 ubuntu 전용 설명을 추가했습니다.

#### 프로젝트 디렉토리 도식화
프로젝트 레포를 디렉토리 별로 간단하게 설명을 붙여봤습니다.

예를 들어 `Main.js`이 메인화면이고 무슨 일을 하는지, `Login.js`가 어떤 정보를 갖고 로그인을 진행하는지 등처럼 말입니다.


### 회고
통상적으로 리드미를 어떻게 구성하면 좋을지 직접 경험해볼 수 있는 시간이었습니다.
물론 정보를 조금 과하게 많이 담았다는 생각은 듭니다.
그래도 덕분에 해당 레포를 참고할 다른 사람을 위해서 설명한다는 식으로 작성하는 것이 어떤 느낌인지 확실히 이해했습니다.
이 작업 거치고 나서 다른 오픈 소스 들의 리드미를 보니 *여기도 이렇게 썻구나* 라는 생각이 절로 느껴졌습니다.