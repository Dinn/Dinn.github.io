---
title: 'Dev Log \| 청소년 쉼터 지도 개발일지 8'
excerpt: 
date: 2020-07-09
last_modified_at: 

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

**진행 기간** 11일

**남은 기간** 1일



## 2020.07.09 Day 11
프로젝트의 마감 하루 전날 입니다.

어제부터 채팅 기능을 2명의 팀원이 붙잡고 있습니다.
팀원에게서 *오늘 밤까지 하면 될 것 같다* 는 말을 들어 해당 기능은 오늘까지 기다려 보려고 합니다.

## 반응형 웹 디자인
모바일 환경에 맞춰 css 설정을 하였습니다.

```js
@media screen and (max-width: 480px) {
  .home {
    flex-direction: column;
  }

  .filter {
    max-width: 100%;
    height: initial;
  }
}
```

거창하게 뭘 설정한 것은 아니고 미디어 쿼리를 이용해서 break point를 설정해주니 다행히도 알아서 잘 배치가 되었습니다.

모바일 화면에서는 위 코드에서 보이는 것처럼 `Filter`와 `Map` 컴포넌트가 세로로 배치되게 설정하고 `Filter`의 `height`는 내용물만큼만 차지하도록 설정했습니다.

어제 CSS를 다시 뜯어고치면서 px, em 등의 단위를 모두 rem으로 통일했습니다.

그 덕분인지 break point만 설정했는데도 알아서 모바일 화면에 맞게 잘 작동되는 것 같습니다.