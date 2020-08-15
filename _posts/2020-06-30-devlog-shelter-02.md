---
title: 'Dev Log \| 청소년 쉼터 지도 개발 일지 2'
excerpt: '기능 정의 / 요구사항 분석 / UI 디자인 / server api 정의 / database scheam 정의'
date: 2020-06-30
last_modified_at: 2020-06-31T00:04:52

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

**진행 기간** 2일

**남은 기간** 10일



## 2020.06.29 Day 2
오늘은 하루종일 팀원들과 회의를 했습니다.

역시 프로젝트에서 제일 어려운 일은 초반 시스템의 범위를 설정하고 요구사항을 분석하는 일인 것 같습니다.

특히 usecases를 다듬고 기능을 명확히 하는데만 반나절 정도를 쓴 것 같습니다.

만들고자 하는 것을 서로가 확실하게 이해하고 각자의 의견을 충분히 논의하여 팀원들이 팀 프로젝트에 참여하는데 무력감을 느끼지 않도록 하는 것이 참 중요하다고 보는 입장에서 꼭 필요한 시간이긴 했습니다.

이 시간들이 남은 프로젝트 기간에 더욱 도움이 되는 시간이었으면 좋겠습니다.



## Requirements
Requirements 작성을 마무리 하였습니다!

### Bare Minimum
1. **Look up map**
    * 지도 상 쉼터의 위치를 보여준다.
2. **Filter shelters**
    * 쉼터 목록을 필터링하여 보여준다.
3. **Show details for a shelter**
    * 쉼터의 이름, 연락처, 주소, 성별, 기간별 유형 정보를 보여준다.
4. **Sign up**
    * 이메일, 이름, 비밀번호, 연락처, 근무 중인 쉼터를 입력하고 회원가입한다.
5. **Sign in**
    * 이메일과 비밀번호를 입력해서 로그인한다.
6. **Sign out**
    * 로그아웃한다.
7. **Chat**
    * 채팅하고 싶은 쉼터를 지정한다.
    * 청소년과 지정된 쉼터 직원은 채팅으로 메세지를 주고 받는다.
    * 이전에 채팅한 기록이 없다면 이름, 나이, 성별, 연락처를 입력한다.

### Advance
1. 지정한 쉼터를 즐겨찾기로 관리한다.
1. 관리자권한을 만들고 관라자(쉼터장)는 직원(staff)을 승인한다.
1. 관리자페이지를 추가한다.
1. 서버가 쉼터 정보를 주기적으로 받아서 데이터베이스에 저장한다.

### 결정 사항
기존의 쟁점들은 다음과 같이 결정됐습니다.
  1. 청소년 유저들은 회원가입 및 로그인을 안하고 채팅할 때 신상정보를 입력한다.
  1. 쉼터 검색은 필터링으로 대체한다.
  1. 서버에서 open api에 요청한 쉼터 정보는 데이터베이스에 저장한다.
  1. 채팅은 지정한 쉼터의 직원을 대상으로 한다.

이 중 3번 사항은 사실 제가 원하는 방향은 아니었습니다.

데이터베이스 따로 관리할 필요도 없고 데이터베이스에서 가져올 필요 없이 실시간 정보를 바로 사용하는 것이 open api의 가장 큰 장점이라 생각하는데 그걸 다시 데이터베이스에 저장하고 사용한다는 것은 open api를 사용하지만 open api가 주는 편의성과 같은 장점은 거부하겠다는 말로 느껴졌기 때문입니다.

그래도 이것을 무작정 나쁜 판단이라고는 보기 힘들 것 같습니다. 데이터 베이스에 저장하면서 staff와의 relation은 더 손쉽게 만들어낼 수 있었고 쉼터 필터링 기능이 더 풍부해졌습니다.

그리고 사실은... 이 api가 2019년 1월 쯤 이후로 업데이트가 없어왔고 [한국청소년쉼터협의회](http://jikimi.or.kr/guide/country_kysa.php)에서 보여주는 자료와 비교했을 때 뒤쳐진 것 같았습니다.

그러니까 데이터 갱신이 매우 드물게 있을 수 있단 얘기고 이렇게 보면 가끔씩 주기적으로 open api에서 받아온 데이터를 데이터베이스의 쉼터 테이블에 다시 넣어주는 것이 싸게 먹힌다고 볼 수도 있겠습니다. *<sub>물론 정석은 아니고 살짝 요령 같긴 합니다.</sub>*


이 외에도 UI, server api, database schema 설계를 마무리했고 각자 맡은 부분에 대해서 tasks를 세운 뒤 본격적인 구현에 들어갈 예정입니다.

[UI](https://snaag818671.invisionapp.com/freehand/Shelter-vA3fNsNLH)

[Server API](https://app.gitbook.com/@bmwismw/s/shelt/v/shelter/)

[DB Diagram](https://dbdiagram.io/d/5efaef610425da461f040bdd)