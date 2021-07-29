---
title: 'Apollo Client의 mutation 이후 refetch 간단 정리'
date: 2021-07-29
last_modified_at: 2021-07-30T03:11:41
excerpt: '개발 중 자주 마주치는 mutation 후 refetch 동작을 apollo client에서 어떤 식으로 제공하고 있는지 간단하게 알아보겠습니다.'

category:
  - Development

tags:
  - React
  - GraphQL
---

본 글은 ![apollo-client](http://img.shields.io/static/v1?label=@apollo/client&message=3.3.11&logo=apollographql&style=flat&color=311C87) 를 기준으로 작성했습니다.

개발하다보면, mutataion을 수행한 이후, 변경 사항을 보여주고자 다시 query를 요청하는 경우를 자주 보게 됩니다.

이럴 때 apollo client는 두 가지의 대표적인 refetching 방식을 소개하고 있습니다.

1. `refetch` 함수
1. `refetchQueries` 옵션

이번 글에선 이에 대해 간단히 살펴보겠습니다.



## refetch
`refetch`는 `useQuery`의 리턴 값인 `QueryResult`의 property 중 하나 이며 이름 그대로 데이터를 다시 한 번 fetch 하는 함수입니다.

`variables`를 optional하게 받을 수는 있지만 기본값은 처음 전달한 `variables` 그대로 입니다.
동일한 `variables`를 사용했을 시 `fetchPolicy`에 따라 cache를 resolve 하는 것을 방지하기 위해 `refetch`는 `network-only`를 기본 `fetchPolicy`로 삼습니다.



## refetchQueries
`useMutation`은 mutation 이후 바로 실행할 query 를 `refetchQueries` 옵션으로 받고 있습니다.
덕분에 `onCompleted`에 할당된 콜백함수에서 `refetch()`를 호출하지 않더라도 mutation 이후 자동으로 리소스를 재요청할 수 있습니다.

`refetchQueries` 옵션은 `query`와 `variable`의 배열을 값으로 갖는데, 자세한 타입 정보는 [공식 문서](https://www.apollographql.com/docs/react/api/react/hooks/#params-2)에서 확인할 수 있습니다.

추가로 `awaitRefetchQueries` 옵션을 통해 `refetchQueries`에 입력한 query의 동기/비동기 여부를 설정할 수도 있습니다.



## networkStatus
재요청을 하다보면 최초 요청과 재요청의 `loading`을 구분해야할 상황이 발생하기도 합니다.

이 때는 `QueryResult`의 `loading` property 대신 `networkStatus`를 통해 현재 네트워크 상태를 자세히 확인할 수 있습니다.

`networkStatus`를 조회하기 전에 우선, `notifyOnNetworkStatusChange` 옵션을 `true`로 바꿔 `networkStatus`를 활성화해줍니다.
`networkStatus`는 네트워크 상태에 따라 1에서 8까지의 값을 가지며, 값의 의미는 다음과 같습니다.

| value | status |
|-------|-------|
| 1 | loading |
| 2 | setVariables |
| 3 | fetchMore |
| 4 | refetch |
| 5 | - |
| 6 | poll |
| 7 | ready |
| 8 | error |


이 중 최초 query는 `1(loading)`, refetch는 `4(refetch)`의 값을 요청이 in flight 인 동안 갖게 되고 이를 통해 요청에 따라 loading을 구분할 수 있는 것입니다.

여기서, 한 가지 주의해야할 점이 있는데, `refetchQueries`로 재요청한 경우엔 `networkStatus` 값이 `4`가 아닌 `1`이 온다는 것입니다.
<sub>(개인적으론 버그 보단 스펙 같습니다.)</sub>
만약 재요청 여부에 따라 loading 중 다른 동작을 부여하고자 할 땐 `refetchQueries` 옵션 보다는 `refetch` 함수를 직접 호출해야겠습니다.

## References
> refetch: [Queries - Client (React) - Apollo GraphQL Docs](https://www.apollographql.com/docs/react/data/queries/#refetching)
> 
> refetchQueries: [Advanced topics on caching - Client (React) - Apollo GraphQL Docs](https://www.apollographql.com/docs/react/caching/advanced-topics/#refetching-queries-after-a-mutation)
>
> networkStatus: [apollo-client/networkStatus.ts](https://github.com/apollographql/apollo-client/blob/d96f4578f89b933c281bb775a39503f6cdb59ee8/src/core/networkStatus.ts#L4)
>
> Apollo Client Hooks API: [Hooks - Client (React) - Apollo GraphQL Docs](https://www.apollographql.com/docs/react/api/react/hooks)
>
> Apollo Client HOC API: [apollo-client/hoc.mdx at main · apollographql/apollo-client](https://github.com/apollographql/apollo-client/blob/main/docs/source/api/react/hoc.mdx#datanetworkstatus)