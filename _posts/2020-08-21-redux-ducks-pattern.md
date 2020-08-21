---
title: 'Redux의 Ducks pattern'
excerpt: '청소년 쉼터 지도에 Redux Ducks Pattern을 적용해봤습니다'
date: 2020-08-21
last_modified_at: 2020-08-12T23:26:31

category:
  - Web

tags:
  - Redux
  - shelter
---

요즘엔 전에 했던 [청소년 쉼터 지도](/portfolio/#청소년-쉼터-지도) 프로젝트를 개선하고 있습니다.

한 달 전에 했던 프로젝트인데 다시 보니 부족한 점이 많이 보이더라구요.
그만큼 배우고 성장했다는 뜻인 것 같아서 기분은 좋습니다.

오늘은 [**ducks pattern**](https://github.com/erikras/ducks-modular-redux)을 참고하여 redux와 관련된 코드를 더 간결하게 수정했는데 이에 대해서 설명하는 글을 남기고자 합니다.
아래에 등장하는 코드들은 실제 청소년 쉼터 지도의 일부입니다.



## 기존의 Redux
redux를 사용하기 위해선 **action**, **reducer**, **store** 가 필요합니다.

```js
// src/actions/index.js
export const SET_FILTER_CONDITIONS = "SET_FILTER_CONDITIONS";

export const setFilterConditions = conditions => ({
  type: SET_FILTER_CONDITIONS,
  conditions,
});
```

```js
// src/reducers/filter.js
import { SET_FILTER_CONDITIONS } from "../actions/index";

export const filter = (state = initialState, action) => {
  switch (action.type) {
    case SET_FILTER_CONDITIONS:
      return Object.assign({}, state, {
        conditions: action.conditions,
      });

    default:
      return state;
  }
};
```

```js
// scr/index.js
import { createStore } from "redux";
import reducers fron "./reducers";

const store = createStore(reducers);
```

redux의 데이터 흐름을 간단히 설명하자면, `src/actions/index.js`에서 액션 객체를 정의하고, `src/reducers/filter.js`의 `filter` 리듀서가 액션에 담긴대로 새로운 state를 만들고, `store`는 새로운 state를 저장하는 식으로 이뤄집니다.
<sub>(리듀서는 `src/reducers/index.js`에서 `combineReducers( )` 함수를 거쳐 하나의 `rootReducer`로 사용 중입니다.)</sub>

기존의 redux는 이렇게 액션과 리듀서를 나누어서 사용하고 있었습니다.

그런데 하나의 상태 `conditions`를 관리하기 위해 이렇게 `actions`와 `reducers` 폴더 양 측에서 코드가 나뉘는 것은 다소 불편한 경험입니다.
실제로 개발 도중 state의 이름을 변경하는 등의 수정사항이 생기면 `import`를 타고타고 이동하면서 `action.type`과 관련된 코드들을 모두 수정해야 합니다.
전에 그렇게 리듀서와 액션을 바꾸려다가 꼬여버려 어디를 고치면 되는지 도저히 못찾아 그냥 원래 코드대로 쓰기로 했던 적도 있을 정도입니다.



## Ducks Pattern
Ducks Pattern도 비슷한 문제에서 고안된 패턴입니다.

리덕스를 사용하기 위해선 항상 `actionTypes`, `actions`, `reducer` 이 셋을 다함께 추가해야하고, <sub>([redux 튜토리얼대로 한다면](https://redux.js.org/basics/actions))</sub> `actions`와 `reducer`를 다른 파일에서 관리하지만 거의 대부분 `actions`와 `reducer`는 하나의 짝이 돼서 함께 동작합니다.

그렇기에 `actionTypes`, `actions`, `reducer`를 **한 곳으로 모아 하나의 모듈로 관리**하자는 것이 Ducks Pattern의 핵심입니다.

참고로 Ducks Pattern의 이름의 유래는 다음과 같습니다.

> ###  Name
>
> Java has jars and beans.
> Ruby has gems.
> I suggest we call these reducer bundles "ducks", as in the last syllable of "redux".



## Ducks Pattern 적용
Ducks Pattern은 다음 4가지 규칙을 따릅니다.

> A module...
> 
> 1. MUST export default a function called reducer()
> 1. MUST export its action creators as functions
> 1. MUST have action types in the form npm-module-or-app/reducer/ACTION_TYPE
> 1. MAY export its action types as UPPER_SNAKE_CASE, if an external reducer needs to listen for them, or if it is a published reusable library

이들 하나하나를 적용해보겠습니다.

### 1. export default a function called reducer()
리듀서를 `reducer` 라는 이름의 함수로 default export 해야한다는 조건입니다.

저는 [redux-actions](https://www.npmjs.com/package/redux-actions)라는 패키지를 이용했기 때문에 정확히 `reducer`란 이름의 함수를 default export 하지는 못했습니다.

redux-actions는 redux의 boilerplate 코드들을 함수로 제공해줍니다.
그래서 기존의 코드를 아래처럼 `handleActions`라는 함수에 reducer 함수 객체를 넘겨주는 식으로 대체할 수 있습니다.

```js
// src/reducers/filter.js
// 변경 전
export const filter = (state = initialState, action) => {
  switch (action.type) {
    case SET_FILTER_CONDITIONS:
      return Object.assign(...);
    default:
      return state;
  }
};

// 변경 후
import { handleActions } from 'redux-actions';

export default handleActions({
  [SET_FILTER_CONDITIONS]: ...
}, initialState)
```

단, `reducer`란 이름으로 export 하기 위해 `const reducer = handleActions(...)` 의 형식으로 코드를 작성한다면 [default export를 할 수 없기 때문에](/javascript/js-module/#default-exports) 바로 `export default handleActions(...)` 하였습니다.


### 2. export its action creators as functions
각 action creator 함수는 export 해야한다는 조건입니다.

이제 action creator는 더이상 다른 파일에 두지 않고 `src/reducers/filter.js`에 함께 써줍니다.

이번에도 redux-actions를 사용해서 함수형태로 export 하지는 않았지만 그래도 action creator는 모두 export 해줬습니다.
redux-actions의 `createAction` 이라는 함수를 사용하면 기존의 코드를 아래와 같이 간단한 코드로 수정할 수 있습니다.

```js
// 변경 전
export const setFilterConditions = conditions => ({
  type: SET_FILTER_CONDITIONS,
  conditions,
});

// 변경 후
import { createAction } from 'redux-actions';

export const setFilterConditions = createAction(SET_FILTER_CONDITIONS);
```

저는 더 나아가서 action creator들을 하나의 객체로 모아 export 해줬습니다.
어차피 사용하는 쪽에서 `filterActions`라는 이름으로 rename해서 사용하기 때문에 애초에 리듀서 파일에서 하나의 객체로 보내는 편을 선택한 것입니다.

```js
// src/reducers/filter.js
export const filterActions = {
  setFilterConditions: createAction(SET_FILTER_CONDITIONS)
}

// src/container/filterCondition.js
// 변경 전
import * as filterActions from '../../reducers/filter'

// 변경 후
import { filterActions } from '../../reducers/filter';
```


### 3. action types in the form npm-module-or-app/reducer/ACTION_TYPE
action type의 네이밍 규칙을 지정하고 있습니다.

조건에 맞춰 action type의 이름을 변경해줍니다.

```js
// 변경 전
const SET_FILTER_CONDITIONS = "SET_FILTER_CONDITIONS"

// 변경 후
const SET_FILTER_CONDITIONS = "shelter/filter/SET_FILTER_CONDITIONS";
```


### 4. export its action types as UPPER_SNAKE_CASE
이 부분은 처음부터 action type을 UPPER_SNAKE_CASE로 작성해왔기 때문에 수정할 부분이 없었습니다.



## Source Code
[source code](https://github.com/Dinn/shelter-public/blob/master/client/src/reducers/filter.js)
```js
// src/reducers/filter.js
// Action Types
const SET_FILTER_CONDITIONS = "shelter/filter/SET_FILTER_CONDITIONS";

// Initail State
const initialState = {
  conditions: {}
}

// Action Creators
export const filterActions = {
  setFilterConditions: createAction(SET_FILTER_CONDITIONS)
}

// Reducers
export default handleActions({
  [SET_FILTER_CONDITIONS]: ...
}, initialState)
```



## 회고
이제는 더이상 `actions` 폴더와 `reducers` 폴더를 오가면서 코드를 보지 않아도 된다는 점에서 이번 리팩토링은 아주 만족스럽습니다.
redux는 유독 코드를 잘게 분리해놔서 한 가지를 수정하고 싶어도 여러 부분을 왔다 갔다 하게 만드는 것 같은데 좋은 해결책을 찾은 것 같습니다.

그리고 수정하는 김에 boilerplate 코드들도 적극적으로 모듈화하여 리듀서가 굉장히 깔끔해졌습니다.

평소 react나 redux 등에서 boilerplate 코드가 자주 등장하는 것이 이들의 진입 장벽을 높인다고 생각했습니다.
형식을 외워야만 이들을 사용할 수 있기 때문입니다.
물론 다른 언어들도 문법을 숙지하는 노력은 필요하지만, 일관된 규칙성을 갖는 약속을 외우는 것과 단순 형식을 외우는 것의 차이는 분명히 존재한다고 생각합니다.

그런 점에서 이번 글과 같은 작업이 react와 redux를 좀 더 사용하기 쉽게 체득해나가는 과정이 될 것입니다.



## References
> [Basic Tutorial: Intro \| Redux](https://redux.js.org/basics/basic-tutorial)
>
> [erikras/ducks-modular-redux: A proposal for bundling reducers, action types and actions when using Redux](https://github.com/erikras/ducks-modular-redux)
