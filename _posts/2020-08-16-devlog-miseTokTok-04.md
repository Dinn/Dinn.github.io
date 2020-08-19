---
title: 'Dev Log \| 미세톡톡 개발 일지 5'
excerpt: 'Toast 메세지 호출 방식 통일화'
date: 2020-08-16
last_modified_at: 2020-08-19T14:22:32
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

## Toast 메세지 호출 방식 통일화
본 프로젝트에서 토스트 메시지는 총 세 가지 유형으로 사용했습니다. 

1. store를 제외한 최상위 레벨에 위치한 컴포넌트에서 특정 state가 변할 때마다 토스트 메시지를 랜더링
1. react-native의 UI component library인 native-base의 root와 toast를 이용
1. 안드로이드 네이티브 토스트를 react-native가 호출

이 중 1번과 2번 유형을 [react-native-root-toast](https://github.com/magicismight/react-native-root-toast) 라이브러리로 대체하고 메시지를 파라메터로 받는 함수를 호출만으로 toast 메시지를 노출시킬 수 있도록 사용 방식을 통일했습니다.