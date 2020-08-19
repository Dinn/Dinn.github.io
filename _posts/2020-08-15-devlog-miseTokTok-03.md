---
title: 'Dev Log \| 미세톡톡 개발 일지 4'
excerpt: 'redux-saga 상태관리 로직 개선'
date: 2020-08-15
last_modified_at: 2020-08-19T14:06:13
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

## redux-saga 상태관리 로직 개선
redux-saga의 중복되는 코드를 모듈화하여 상태 관리 로직을 개선하는 작업을 하였습니다. 

Redux-saga는 redux의 미들웨어 중 하나로써 액션이 발생했을 때 수반하고픈 side-effect를 일으켜주는 역할을 합니다. 

특히 saga는 redux의 액션과 함께 API 요청을 하는데 아주 유용합니다.  

Redux의 액션은 순수 함수이기 때문에 결과의 일관성이 보장되어야 합니다.
이를 위해 동일한 입력값이 동일한 결과값으로 이어지지 못하게 하는 api요청과 같은 side effect들은 분리하여 saga에서만 실행되도록 하는 것입니다. 

제가 찾은 중복되는 코드 또한 다음과 같이 API 요청을 처리하는 과정에서 발생했습니다. 

```js
try {
  // fetch 시작
  // 서버에서 데이터를 수신
  // 받은 데이터를 state에 저장
} catch(err) {
  // 에러 발생 시 state에 에러 객체를 저장
  // 발생한 에러를 모니터링 툴에 보고
} finally {
  // fetch 종료
}
```

프로젝트의 대부분의 이펙트들은 try구문에서 데이터를 받아오고 저장하며 에러가 발생했을 시 catch 구문에서 로깅하는 형식을 따랐습니다. 

이를 하나의 함수로 만들어 데이터를 받아와 사용자가 state에 저장하는 부분만을 코드로 작성하도록 하였습니다. 

```js
function* sagaFetch(callbackFn) {
  try {
    // fetch 시작
    yield call(callbackFn);
  } catch(err) {
    // 에러 발생 시 state에 에러 객체를 저장
    // 발생한 에러를 모니터링 툴에 보고
  } finally {
    // fetch 종료
  }
}
```



## 회고
saga코드를 충분히 접하면서 많은 공부가 되었습니다.
한편, `try...catch...finally` 구문을 모듈화하는 것이 최선인지도 계속 고민을 하였습니다.
코드 리뷰 해주신 멘토분은 현 코드 상황에선 그래도 이정도면 최선이라고 말씀해주셨지만 이유모를 찝찝함이 남아있습니다.
또 saga에서가 아니라 reducer에서 exception handling을 하는 방법으로도 연습을 해봐야겠습니다.