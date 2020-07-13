---
title: 'Dev Log \| 청소년 쉼터 지도 개발 일지 1'
excerpt: 'Sotfware Requirements 정의 / 역할 분배 / 기능 리스트업 '
date: 2020-06-29
last_modified_at: 2020-06-30T01:41:42

category:
 - Development Logs

tags:
 - Development Logs
 - Dev Log
 - shelter
---

**주제** Shelter: 청소년 쉼터 지도

**프로젝트 기간** 2020.06.29 - 2020.07.10 (12일)

**진행 기간** 1일

**남은 기간** 11일

**프로젝트 레포** [https://github.com/codestates/shelter](https://github.com/codestates/shelter)



## 2020.06.29 Day 1
프로젝트의 첫날 팀원들과 간단한 인사를 하고 프로젝트를 시작하였습니다.

아직 무언가를 구현하지는 않고 준비 운동만 하다가 하루가 끝난 것 같아서 벌써부터 시간을 낭비해버린 기분입니다.ㅠㅠ

오늘 진행 사항을 정리하겠습니다.

## Team Rules 설정
프로젝트를 진행하는 동안 지킬 규칙들을 설정했습니다.

간단한 commit, pull request 규칙들과 시간 약속 등을 정했는데 commit rules에 관해선 내일 더 구체적으로 한번 더 정할 예정입니다.

commit rules에 관해선 아래 글을 따라가면 좋을 것 같습니다.

[Git Commit Message Rule](https://hyeran-story.tistory.com/99)

*생각해보니 코드 규칙를 빼먹어서 내일 esLint 사용을 점검해봐야 할 것 같습니다.*

## Usecases Details
어제 정의한 usecases를 더 자세하게 분석하여 task card 작성할 준비를 하겠습니다.

* Look up map
* Search shelters
  * 검색 기능이 없어도 되는 이유
    1. 어차피 open api의 매개변수는 지역 밖에 없기 때문에 검색을 할려면 전체 데이터를 서버에 올려놓고 필터링 해야한다.
    1. 지도를 띄워주는 것이지 google 지도, naver 지도가 아니기 때문에 지도에서 다른 장소를 검색할 이유가 없다.
    1. 쉼터를 검색하기 위해 검색어를 입력한다고 해도 어차피 검색어는 쉼터 이름 아니면 `쉼터` 그 자체일텐데 그 긴 쉼터 이름을 기억하고 입력하느니 지역별 2~3개 있는 쉼터를 직접 보는 것이 낫다.
    1. 검색 기능 말고도 지역별로 필터링할 수 있도록 체크박스/드롭박스를 두는 대안이 있다.
* Show details for a shelter
  * 보여줄 정보: 쉼터명, 연락처, 도로명주소, 성별유형, 기간별 유형, *운영기관명
* Chat
* Sign in
  * email, password 입력
* Sign out
* Sign up
  * email, password, 이름, 근무지 입력
  * 근무지는 지역 선택하면 지역별로 보여주는 목록 중에서 선택한다.
* Approve staff

## Sprint 2: 쉼터 지도 구현 및 1차 배포 (96h)
* client
  * client 개발 환경 구축 0.5h

* server
  * server 개발 환경 구축 0.5h

* database
  * table 작성 1h
  * 개발용 더미 데이터 추가 0.5h

적다보니 생각났는데 이 이상 진행하기 위해선 사전 작업이 필요해 보입니다.

client에선 **wireframe**이, server에선 **server api**가, database에선 **database diagram**이 준비돼야 하는데 급한 마음에 일단 task부터 정하려고 했으니 생각이 막힐 수 밖에 없었던 겁니다.

## 문제 및 보완
Requirements를 정의하면서 급한 마음에 클라이언트, 서버, 데이터베이스 구조를 충분히 고려하지 않고 바로 task 부터 나누려고 해서 진행이 막혔다.
1. Software Requirments의 task를 정리한다.
1. 클라이언트, 서버, 데이터베이스 설계를 충분히 고려하고 완료한다.
  * 단, 개발 경험이 거의 없는 만큼 처음부터 완벽한 설계는 불가능할 것이다. 
  * 현실적으로 개발 도중 설계가 어느 정도는 수정될 것임을 염두에 두고 진행한다.
1. 설계를 바탕으로 task card를 작성한다.