---
title: 'Dev Log \| 미세톡톡 개발 일지 6'
excerpt: 'Error Boundary 설정 및 Fallback UI 적용'
date: 2020-08-17
last_modified_at: 2020-08-19T15:39:55
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

## Error Boundary
마지막으로 프로젝트에 Error Boundary를 설정하고 Fallback UI를 적용했습니다.

아래 영상은 코드에 throw new Error() 코드를 넣어 에러가 발생했을 때 기존 앱이 어떻게 반응하는지 보여줍니다.

<div style="text-align:center;">
  <img src="/assets/images/devlog-miseTokTok/whiteScreen.gif" width="30%" />
</div>
<br>

보시는 바와 같이 기존 앱에선 에러가 발생하면 빈 화면만 노출될 뿐 앱 내에서 자체적으로 아무런 조치를 취할 수 없습니다.

이를 해결하기 위해 Error Boundary를 설정했습니다.

**Error Boundary**란, 하위 컴포넌트 트리의 어디서든 자바 스크립트 에러를 기록하며 깨진 컴포넌트 트리 대신 Fallback UI를 보여주는 React 컴포넌트입니다.

Error Boundary 컴포넌트는 다음 세 메소드를 가집니다.

1. **componentDidCatch**
: Error Boundary 컴포넌트에서 에러의 유무를 판단하는 상태인 `hasError`를 `true`로 변경
1. **getDerivedStateFromError**
: Error 리포팅을 하는 등의 사이드 이팩트를 수행
1. **render**
: `return hasError ? <Fallback /> : this.props.children`

먼저  메소드가 에러 유무를 판단하는 state를 변경하고,  함수가 에러 리포팅 등을 하면, render 함수는 에러 유무를 판단하는 state에 따라 Fallback UI나 정상적인 컴포넌트를 랜더링합니다.

Fallback UI는 문제 사항을 개발자에게 전달할 수 있도록 메일을 보내거나 메일 주소를 복사할 수 있는 기능을 탑재하였고 깨진 컴포넌트에서 벗어나기 위해 초기 화면으로 돌아가는 버튼을 갖고 있습니다.
초기화면은 로그인 여부에 따라 다른 화면으로 이동합니다.

<div style="text-align:center;">
  <img src="/assets/images/devlog-miseTokTok/intro.gif" width="32%" />
  <img src="/assets/images/devlog-miseTokTok/home.gif" width="32%" />
  <img src="/assets/images/devlog-miseTokTok/mail.gif" width="32%" />
</div>