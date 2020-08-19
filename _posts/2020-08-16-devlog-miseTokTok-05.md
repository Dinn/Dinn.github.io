---
title: 'Dev Log \| 미세톡톡 개발 일지 5'
excerpt: 'Toast 메세지 호출 방식 통일화'
date: 2020-08-16
last_modified_at: 2020-08-19T14:22:32

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
1. react-native의 UI component library인 [native-base](https://nativebase.io/)의 root와 toast를 이용
1. 안드로이드 네이티브 토스트를 react-native가 호출

이 중 1번과 2번 유형을 [react-native-root-toast](https://github.com/magicismight/react-native-root-toast) 라이브러리로 대체하고 메시지를 파라메터로 받는 함수를 호출만으로 toast 메시지를 노출시킬 수 있도록 사용 방식을 통일했습니다.



## react-native-root-toast
react-native-root-toast 라이브러리는 `showToast( )` 함수를 호출하기만 하더라도 토스트 메세지가 나타나서 매우 간단해 보입니다.

하지만 코드를 분석해보면 토스트를 보여주는 과정이 그렇게 간단한 건 또 아닙니다.

`showToast` 함수가 호출되면 `root` 컴포넌트의 sibling 컴포넌트를 임시로 만들고, 그 sibling 컴포넌트에서 토스트 메세지를 보여준 다음에, 토스트 메세지 애니메이션이 끝나고 메세지가 사라지면 sibling 컴포넌트도 같이 사라지는 것 같습니다.

native-base에서도 `Toast.show( )`를 사용하기 위해선 컴포넌트를 `<root>...</root>`로 감싸야 합니다.

두 사례로 미뤄보아 토스트 메세지를 커스텀해서 띄우기 위해선 toast를 보여줄 최상위 컴포넌트를 애초에 만들고 그 컴포넌트에서 화면에 레이어를 씌우듯이 토스트를 띄우는 것이 정석인듯 보입니다.



## 회고
toast를 react-native-root-toast 라이브러리 통째로 사용해서 토스트 메세지의 스타일을 커스텀할 수 없었던 점이 많이 아쉽습니다.
그래도 라이브러리를 분석하여 toast를 어떤 식으로 띄울 수 있는지 직접 공부하는 기회가 되었고 다음엔 처음부터 끝까지 커스텀 토스트를 만드는 것에도 도전해보고 싶습니다.
제 손으로 처음부터 끝까지 다 만드는 것도 의미가 있고 좋았겠지만, 라이브러리를 사용하는 것의 장점도 분명히 있습니다.
그만큼 시간과 노력을 아끼고, 코드가 복잡해지는 것도 예방할 수 있으니까요.



## References
> [magicismight/react-native-root-toast: react native toast like component, pure javascript solution](https://github.com/magicismight/react-native-root-toast)
>
> [NativeBase \| Essential cross-platform UI components for React Native](https://nativebase.io/)