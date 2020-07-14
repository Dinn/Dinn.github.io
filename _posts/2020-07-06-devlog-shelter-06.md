---
title: 'Dev Log \| 청소년 쉼터 지도 개발일지 6'
excerpt: 'FilterList와 ShelterMarker 상태 공유'
date: 2020-07-06
last_modified_at: 2020-07-14T18:29:44

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

**진행 기간** 8일

**남은 기간** 4일



## 2020.07.06 Day 8
오늘부터 Sprint 3에 들어가 로그인 기능과 채팅을 구현할려고 했지만... 하다보니 어제 남은 `FilterItemDetail` 스타일 설정을 마무리하고 `Map` 컴포넌트와 연결하는 *나머지 작업* 을 하게 됐습니다.

## Filter와 Map 연결
![test](https://user-images.githubusercontent.com/35102081/87310884-873e7500-c559-11ea-97ce-5079b48d765b.gif)

원했던 세부기능은 다음과 같습니다.

* 목록에서 쉼터를 클릭하면 쉼터의 상세 정보를 보여준다.
  * 다른 쉼터의 상세 정보를 이미 보여주고 있다면 다른 쉼터의 상세 정보를 감추고 지금 선택한 쉼터의 상세정보를 보여준다.
  * 쉼터와 대응되는 지도 상 마커에서도 정보 창을 보여준다.
  * 정보 창이 떠있는 쉼터가 화면 안에 들어오도록 지도가 보여주는 지역을 변경한다.
* 지도 위의 마커를 클릭하면 정보 창이 뜬다.
  * 쉼터 목록에서 대응하는 항목의 상세 정보도 노출된다.
* 정보 창이 뜬 상태에서 다시 마커를 클릭하면 정보 창이 닫힌다.
* 정보 창 우측 상단에 닫기 버튼을 눌러서 정보창을 닫을 수도 있다.

이를 구현하기 위해서 목록의 각 항목과 마커 컴포넌트에 쉼터 데이터를 지닌 `shelter` 객체를 상태로 갖게 하고 redux store에 `currentShelter` 라는 상태를 만들었습니다.

`currentShelter`는 다음과 같이 변경됩니다.
* 목록이나 마커를 클릭할 때 `currentShelter`가 비어있다면 클릭된 target component가 갖고 있는 `shelter`를 `currentShelter`에 `dispatch`한다.
* 목록이나 마커를 클릭할 때 `currentShelter`와 클릭된 target component가 갖고 있는 `shelter`가 갖다면 `currentShelter`에 빈 객체를 `dispatch`한다.
* 목록이나 마커를 클릭할 때 `currentShelter`와 클릭된 target component가 갖고 있는 `shelter`가 다르다면 `currentShelter`에 선택된 컴포넌트의 `shelter` 객체를 `dispatch` 한다.

그리고 세부 정보와 마커는 `&&` 연산자를 사용해서 `shelter === currentShelter` 란 조건을 만족했을 때만 랜더링 되도록 하였습니다.

```js
// FilterItem
  // ...
  render() {
    // ...
    return (
      <div>
        
        {this.props.shelter === this.props.currentShelter && (
          <FilterItemDetail shelter={this.props.shelter} />
        )}
      </div>
    );
  }
  // ...
```


### 문제 및 보완
#### Filter와 Map 컴포넌트의 구현 방식 차이
`Filter` 컴포넌트 담당은 저였고 `Map` 컴포넌트는 다른 팀원의 몫이었습니다.

그런데 컴포넌트를 다 만들고 보니 `Filter`는 class로, `Map`은 function으로 구현되어 있었습니다.
컴포넌트를 class로 할지 function으로 할지에 대해 시작할 땐 통일해야하는 필요성을 크게 못느끼고 별다른 얘기가 없어서 발생한 문제 같습니다.

둘을 이을려고 하다보니 한다면 할 수 있겠지만 마치 한 프로그램에서 다른 언어를 쓰는 것 마냥 불편감이 상당했습니다.
이제서야 하나의 형식으로 리팩토링하기엔 시간이 없어 그대로 했지만 나중에 또다른 프로젝트를 한다면 컴포넌트 형태를 통일하는 규칙도 필요해 보입니다.

#### shelter 공유
문제점까지는 아닌 것 같고, store에 `shelter` 객체 전체를 저장하지 말고 `id` 값을 저장하는 방법도 되돌아 보니 떠올랐습니다.

구현할 당시는 미처 생각 못한 바람에 `id`를 갖도록 나중에 바꿔볼려고 했으나 action 객체와 reducer를 모두 바꾸는게 생각보다 복잡해서 그냥 그대로 뒀습니다.

그런데 또 생각해보면 `shelter` 객체 전체를 저장한다고 해도 어차피 pass by reference 이기 때문에 기존 방식이 크게 비효율적이거나 하진 않은 것 같습니다.