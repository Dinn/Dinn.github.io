---
title: 'Dev Log \| 청소년 쉼터 지도 개발일지 7'
excerpt: '시작 위치 설정'
date: 2020-07-08
last_modified_at: 2020-07-14T22:10:55

category:
  - Development Logs

tags:
  - Development Logs
  - Dev Log
  - shelter
---

**주제** Shelter: 청소년 쉼터 지도

**프로젝트 레포** [https://github.com/codestates/shelter](https://github.com/codestates/shelter)

**프로젝트 기간** 2020.06.29 - 2020.07.10 (12일)

**진행 기간** 10일

**남은 기간** 2일



## 2020.07.08 Day 10
주말에 팀원분이 [jwt](https://jwt.io/)를 이용해 로그인 api를 완성해주셨습니다.

덕분에 로그인은 프론트 엔드에서만 하면 되는 상황이었고 advance requirements에 도전해보고 싶었습니다.

그래서 OAuth 인증 기능을 탑재해서 구글 계정으로 로그인 하도록 만드는 기능을 목표로 삼았습니다.

그런데 몇 가지 문제가 있어서 결국 포기할 수 밖에 없었습니다.

## OAuth 2.0 적용
우선 클라이언트 단에서 구글 로그인 UI를 간단히 만들 순 있지만 우리가 원하는 건 서버를 거쳐 인증하는 방법이었습니다.

정확하진 않지만, 클라이언트에서 이메일, 비밀번호를 구글 계정 인증 서버에 보내고 인증 서버는 토큰 값을 우리가 개발한 서버에 보내주는 플로우로 인증이 이뤄지고 회원가입 시 필요한 정보는 직접 개발한 서버에서 인증 서버로 요청해서 받아오는 걸로 이해했습니다.

그런데 포기했던 이유는 바로 이런 인증 과정이 **https**에서만 가능하기 때문입니다.

이번 프로젝트는 http 기반으로 서버를 구축했기 때문에 이미 구현된 jwt 방식 로그인으로 만족하기로 하고 로그인 UI를 만드는 것에 집중하기로 했습니다.

그러면서 jwt 인증 api를 만들어 주셨던 [bombamong](https://github.com/bombamong)님이 로그인 UI를 만들어주시고 저는 전체적인 스타일링을 하며 반응형 웹 디자인을 하여 모바일에서도 최적화된 화면을 볼 수 있도록 역할 분배를 했습니다.



## 시작 위치 설정
초기 화면에서 현재 위치를 지도에 띄우고 싶었습니다.

구글링해보니 `navigator.geolocation.getCurrentPosition()`로 브라우저에서 디바이스의 현재 위치를 가져올 수 있었습니다.

문제는 `getCurrentPosition()`는 프로미스 객체를 리턴하는 비동기 함수이기에 현재위치를 `Map` 컴포넌트에 넣기 위해선 콜백함수 내부에서 상태 관리를 해줘야 했습니다.

업친 데 덮친 격으로 `Map` 컴포넌트는 함수형이기 때문에 hooks까지 고려를 해야 했습니다.

### `navigator.geolocation.getCurrentPosition()` 내부에서 현재 위치 상태를 초기화
```js
const Map = props => {
  navigator.geolocation.getCurrentPosition(pos => {
    const [curPos, setCurPos] = useState({
      lat: pos.coords.latitude,
      lng: pos.coords.longitude,
    })
  });

  return (
    <div className="map">
      <MapWithAMarker
        defaultCenter={curPos}
      />
    </div>
  );
};
```
우선 `useState`는 최상위 range가 아니면 사용할 수 없기 때문에 아예 문법적으로 틀린 접근법입니다.

### `navigator.geolocation.getCurrentPosition()` 내부에서 현재 위치 상태를 변경
```js
const Map = props => {
  const [curPos, setCurPos] = useState({});
  
  navigator.geolocation.getCurrentPosition(pos => {
    setCurPos({
      lat: pos.coords.latitude,
      lng: pos.coords.longitude,
    });
  });

  return (
    <div className="map">
      <MapWithAMarker
        defaultCenter={curPos}
      />
    </div>
  );
};
```
`setState()` 같이 상태 변경을 랜더링하는 함수 안에 두면 `랜더링 -> 상태 변경 -> 변경된 상태로 다시 랜더링 -> 상태 변경 -> 다시 랜더링 -> ... `의 순서로 무한 루프에 빠지게 됩니다.

결국 제가 사용한 방법은 조건문을 사용해서 `curPos`에 이미 값이 있을 땐 `setCurPos`를 하지 않는 방법이었습니다. `랜더링 -> 상태변경 -> 변경된 상태로 다시 랜더링` 에서 멈추는 것이죠.

```js
const Map = props => {
  const [curPos, setCurPos] = useState({});

  if (Object.keys(curPos).length === 0) {
    navigator.geolocation.getCurrentPosition(pos => {
      setCurPos({
        lat: pos.coords.latitude,
        lng: pos.coords.longitude,
      });
    });
  }

  return (
    <div className="map">
        <MapWithAMarker
          defaultCenter={curPos}
        />
    </div>
  );
};
```

> react의 life cycle methods를 이용하여 이 문제를 해결하는 것이 정답에 가까워 보입니다.
>
> 당시엔 `componentDidMount`, `componentWillMount` 등을 hooks를 사용해서 어떻게 써야할지 몰라서 저런 안티 패턴이 나온 것 같습니다.
> 
> 덕분에 life cycle methods와 useEffect의 필요성을 몸소 느끼는 경험이 되었습니다.