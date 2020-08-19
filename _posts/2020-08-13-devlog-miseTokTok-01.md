---
title: 'Dev Log \| 미세톡톡 개발 일지 2'
excerpt: '클래스형 컴포넌트를 함수형 컴포넌트로 리팩토링'
date: 2020-08-13
last_modified_at: 2020-08-19T09:07:57

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

## React Component
React에서 컴포넌트는 함수와 클래스 두 가지 형태로 정의할 수 있습니다.

클래스형 컴포넌트는 lifecycle method와 state를 사용할 수 있다는 점에서 함수형 컴포넌트와 비교됩니다.

하지만, 16.8 버전부터 **hooks**라는 개념이 도입되면서 함수형 컴포넌트에서도 lifecycle이나 state와 관련된 작업들을 처리할 수 있게 됐습니다.
<sub>*(클래스 컴포넌트에서만 가능한 몇 가지 기능들이 아직 남아있다고는 합니다.)*</sub>

아직 많이 사용해보진 않아서 항상 그렇다고는 확신할 수 없지만 그래도 hooks를 사용했을 때 코드가 좀 더 간단해지는 것 같아서 hooks의 도입을 굉장히 긍정적으로 생각하고 있었고 이번 요구사항도 클래스로 구현된 컴포넌트들을 함수형으로 리팩토링 하는 것이었습니다.



## Hooks in React Redux
![react-redux](/assets/images/devlog-miseTokTok/react-redux.png)

React에서 Redux를 사용하기 위해선 외양을 담당하는 presentational component와 데이터의 읽고 쓰기를 담당하는 container component를 `connect( )` 함수로 묶어줘야 합니다.

하나의 component를 두 개의 sub-component로 나눈다는 것은 다른 기능을 수행하는 요소를 분리한다는 측면에선 긍정적으로 바라볼 수 있겠습니다.

하지만, 하나의 컴포넌트를 위한 소스 코드가 분산되고, 때문에 코드의 유지보수에 어려움이 생기는 것 또한 사실입니다.
새로운 state를 dispatch하고 싶다면 presentational component와 container component 두 부분을 모두 수정해야 한다는 점을 예로 들 수 있겠습니다.

이런 불편함은 `useSelector`, `useDispatch` hooks를 이용하여 해소할 수 있습니다.
`connect( )` 등을 사용하지 않고도 `useSelector`로 state를 읽어올 수 있고 `useDispatch`로 state를 저장할 수 있기 때문입니다.



## Component Refactoring
다음은 실제 리팩토링했던 컴포넌트의 일부분입니다.

```js
// presentational component
class DailyTokTok extends Component {
  // ...
  
  const {
    poll,
    weeklyResults,
    fetchButtonDisabled,
    submitPoll,
    handleButtonState,
  } = this.props
  
  // ...
}

// container component
const mapStateToProps = state => {
  poll: state.poll.polls[0],
  weeklyResults: state.poll.weeklyResults,
  fetchButtonDisabled: state.poll.fetchButtonDisabled,
  // ...
}

const mapDispatchToProps = dispatch => {
  submitPoll: (pollId, value, latitude, longitude) =>	
    dispatch(pollCreators.submitPoll({ pollId, value, latitude, longitude })),
  handleButtonState: isButtonDisabled =>	
    dispatch(pollCreators.handleButtonState(isButtonDisabled)),
  // ...
}

export default connect(mapStateToProps, mapDispatchToProps)(DailyTokTok);
```

container component의 `mapStateToProps`와 `mapDispatchToProps`가 store에서 component로 보내고 받을 state를 지정합니다.
또 이렇게 container에서 처리를 해줬기 때문에 store의 state를 presentational component에서 접근할 수 있는 것입니다.

`useSelector`와 `useDispatch`를 사용하여 위 코드를 아래와 같이 리팩토링 했습니다.

한눈에 보더라도 상당히 간단해졌습니다.

```js
export default function DailyTokTok({ navigation }) {
  const dispatch = useDispatch();
  const submitPoll = (pollId, value, latitude, longitude) =>
    dispatch(pollCreators.submitPoll({ pollId, value, latitude, longitude }));
  const handleButtonState = isButtonDisabled =>
    dispatch(pollCreators.handleButtonState(isButtonDisabled));

  const {
    polls: [poll],
    weeklyResults,
    fetchButtonDisabled,
  } = useSelector(state => state.poll);

  // ...
}
```



## Optimization
더 나아가 `useCallback`을 사용하여 `dispatch`를 최적화할 수 있습니다.

```js
export default function DailyTokTok({ navigation }) {
  const dispatch = useDispatch();
  const submitPoll = useCallback((pollId, value, latitude, longitude) =>
    dispatch(pollCreators.submitPoll({ pollId, value, latitude, longitude })),
    [dispatch, pollCreators]
  )
  const handleButtonState = useCallback(isButtonDisabled => 
    dispatch(pollCreators.handleButtonState(isButtonDisabled)),
    [dispatch, pollCreators]
  ) 

  const {
    polls: [poll],
    weeklyResults,
    fetchButtonDisabled,
  } = useSelector(state => state.poll);

  // ...
}
```

`useCallback`은 두 번째 인자로 배열을 받습니다.
컴포넌트가 리랜더링 되더라도 배열의 원소의 값이 변하지 않는다면 `useCallback`은 리로드되지 않는 점을 이용해 불필요한 랜더링을 막을 수 있습니다.



## References
> [Usage with React \| Redux](https://redux.js.org/basics/usage-with-react#presentational-and-container-components)
>
> [Hooks · React Redux](https://react-redux.js.org/api/hooks)