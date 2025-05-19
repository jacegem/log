---
title: useReducer
tags:
categories:
date: 2025-03-25
lastMod: 2025-05-18
---






useReducer 는 상태와 상태를 조작하기 위한 수단(함수)을 모두 관리한다.



useState는 단일 상태를 관리하는데 적합하고 [useReducer]({{< ref "/pages/useReducer" >}})는 복잡한 상태를 관리한다는 점에서 다릅니다.

```typescript
import { useReducer } from 'react'

const initialState = {
  count: 0,
  name: "Tejumma",
  age: 30,
}

const reducer = (state, action) => {
  switch (action.type) {
    case 'increment':
      return {...state, count: state.count + 1}
    default:
      return state
  }
}

const MyComponent = () => {
  const [state, dispatch] = useReducer(reducer, initialState)
  
  return (
    <div>
      <p>카운트: {state.count}</p>
      <p>이름: {state.name}</p>
	  <p>나이: {state.age}</p>
	  <button onClick={()=>dispatch({type:'increment'})}>증가</button>
    </div>
  )
}
```



useReducer를 사용할 때 얻는 이점 세 가지

상태 업데이트 로직을 컴포넌트에서 분리합니다.

함께 제공되는 reducer 함수는 단독으로 테스트하고 다른 컴포넌트에서도 재사용이 가능합니다.

이 방법은 컴포넌트를 깔끔하고 단순하게 유지하고 단일 책임 원칙을 따르기에 좋습니다.

리듀서는 다음과 같이 테스트할 수 있습니다.

```typescript
describe('reducer', ()=>{
  test('증가 액션이 주어지면 카운트가 증가해야 합니다', ()=>{
    const initialState = {
      count: 0,
      name: 'my-name',
      age: 30,
    }
    const action = { type: 'increment' }
    const expectedState = {
      count: 1, 
      name: 'my-name'
    }
    const actualState = reducer(initialState, action)  // 위에서 작성한 reducer
    expect(actualState).toEqual(expectedState)
  })
  
  test('알 수 없는 액션이 주어지면 동일한 객체를 반환해야 합니다', ()=>{
    const initialState = {
      count: 0,
      name: 'my-name',
      age: 30,
    }
    const action = { type: 'increment' }
 	const expectedState = initialState
    const actualState = reducer(initialState, action)  // 위에서 작성한 reducer
    expect(actualState).toEqual(expectedState)
  })
})
```

상태와 상태 변경 방식은 항상 명시적으로 useReducer와 함께 사용됩니다. 어떤 이들은 useState가 JSX트리 계층을 통한 컴포넌트 상태 업데이트의 전반적인 흐름을 파악하기 어렵게 만들 수 있다고 주장하기도 합니다.

useReducer는 이벤트 소스 event source 모델입니다. 즉, 애플리케이션에서 발생하는 이벤트를 모델링해서 일종의 진단 로그르 추적하는 데 사용할 수 있다는 의미입니다.

이 진단 로그는 애플리케이션의 이벤트를 재생해 버그를 재연하거나 시간 여행 디버깅을 구현하는 데 사용할 수 있습니다.

또 실행 취소, 재실행, 낙관적 업데이트, 인터페이스 전반의 일반적인 사용자 행동에 대한 분석 추적 같은 강력판 패턴도 사용 가능합니다.




