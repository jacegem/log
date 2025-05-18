---
title: immer
tags:
categories:
date: 2025-01-03
lastMod: 2025-05-18
---




인기 있는 리액트 라이브러리인 Immer는 애플리케이션에서 복잡한 상태 관리를 처리할 때 특히 유용합니다.

상태가 중첩되거나 복잡한 형태라면, 기존에 사용하던 상태 업데이트 방식은 장황하고 오류가 발생하기 쉽습니다.

Immer는 변경 가능한 초안 상태로 작업하고, 한 번 생성된 상태는 변경 불가로 만들어서 복잡성을 관리할 수 있도록 도와줍니다.



```typescript
import { useImmerReducer } from 'use-immer'

const initialState = {
  user: {
    name: 'John Doe',
    age: 28,
    address: {
      city: 'New York',
      country: 'USA'
    }
  }
}

const reducer = (draft, action) => {
  switch (action.type) {
    case 'updateName':
      draft.user.name = action.payload
      break
    case 'updateCity':
      draft.user.address.city = action.payload
      break
    default:
      break
  }
}

const MyComponent = () => {
  const [state, dispatch] = useImmerReducer(reducer, initialState)
  //...
}
```



Immer 는 현재 상태와 일련의 명령어를 기반으로 다음 상태를 생성하는 데 사용하는 produce 함수를 제공합니다.

```typescript
import produce from 'immer'
import { useState } from 'react'

const MyComponent = () => {
  const [state, setState] = useState(initialState)
  
  const updateName = (newName) => {
    setState(
      produce((draft) => {
        draft.user.name = newName
      })
    )
  }
  //...
}
```







```typescript
export const immer = (config: any) => (set: any) => {
  return Object.entries(config()).reduce(
    (acc, [key, value]) => ({
      ...acc,
      [key]:
        typeof value === "function"
          ? (...args: any[]) => set(produce(draft => void config(draft)[key](...args)))
          : value
    }),
    {}
  );
};
```
