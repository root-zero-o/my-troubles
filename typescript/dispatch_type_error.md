# dispatch type 오류
![error2](https://user-images.githubusercontent.com/97326130/177174536-465e8a61-97c9-4cde-9a73-5ccd733ca760.png)

## 문제 상황
```javascript
import { TodoState } from "./../type";
import { AnyAction, ThunkDispatch } from "@reduxjs/toolkit";
import { useEffect } from "react";
import { useDispatch, useSelector } from "react-redux";
import { RootState } from "../store/modules";
import { fetchTodos } from "../store/modules/todosSlice";

export default function useTodos() {
  const dispatch = useDispatch();  

  useEffect(() => {
    dispatch(fetchTodos());  // 여기 fetchTodos()에서 오류 발생 !
  }, [dispatch]);

  const { lists, loading, error } = useSelector(
    (state: RootState) => state.todos
  );
  return { lists, loading, error };
}
```
- redux toolkit을 사용하다가 오류가 발생했다😅
- ```useDispatch```에 타입이 필요한 것 같았는데.. 스택오버플로우에서 찾은대로 해도 해결이 되질 않았다 ..
- 하지만 킹왕짱 king of typescript 준호님이 해결해주셨다.

## 해결1
```javascript
// store/modules/todosSlice.ts

import { TodoState } from "./../type";
import { AnyAction, ThunkDispatch } from "@reduxjs/toolkit";
import { useEffect } from "react";
import { useDispatch, useSelector } from "react-redux";
import { RootState } from "../store/modules";
import { fetchTodos } from "../store/modules/todosSlice";

type AppDispatch = ThunkDispatch<TodoState, any, AnyAction>;  // useDispatch가 아니고 ThunkDispatch 사용

export default function useTodos() {
  const dispatch: AppDispatch = useDispatch();  // 타입을 지정해준다.

  useEffect(() => {
    dispatch(fetchTodos());
  }, [dispatch]);

  const { lists, loading, error } = useSelector(
    (state: RootState) => state.todos
  );
  return { lists, loading, error };
}
```
- redux thunk는 컴파일될 때 ```ThunkAction```을 빌드하는데, 그 때 타입이 필요하다.(TS는 이걸 못참는다...)
- 그래서 ThunkDispatch를 이용해 타입을 지정해 주었다.
- ```ThunkDispatch<state, extra argument, action>``` 형태로 타입 지정

## 해결2
- 해결 전 코드
```javascript
//store/configStore.ts

import { configureStore, getDefaultMiddleware, MiddlewareArray } from '@reduxjs/toolkit';
import { createWrapper } from 'next-redux-wrapper';
import rootReducer from './modules';


  export const makeStore = () => configureStore({ 
    reducer : rootReducer,
    middleware: [...getDefaultMiddleware()] 
  })

export type AppStore = ReturnType<typeof makeStore>

const wrapper = createWrapper<AppStore>(makeStore)
  
export default wrapper;
```
- 해결 후 코드
```javascript
import { configureStore, getDefaultMiddleware, MiddlewareArray } from '@reduxjs/toolkit';
import { createWrapper } from 'next-redux-wrapper';
import rootReducer from './modules';


  export const makeStore = () => configureStore({ 
    reducer : rootReducer,
    middleware: (getDefaultMiddleware) => getDefaultMiddleware()
    // 스택오버플로우 + 공식문서에서 미들웨어를 이런 형태로 써준 걸 보고, 따라 고쳤더니 해결되었다.
    // 왜 해결된건지는 아직 모르겠다 ..😅
  })

  export type AppStore = ReturnType<typeof makeStore>

const wrapper = createWrapper<AppStore>(makeStore)
  
export default wrapper;
```

### 참고
- https://redux.js.org/usage/usage-with-typescript/#usage-with-redux-thunk
- https://redux-toolkit.js.org/api/getDefaultMiddleware
- https://stackoverflow.com/questions/70143816/argument-of-type-asyncthunkactionany-void-is-not-assignable-to-paramete/72468596#72468596?newreg=a01d8ba1317d4e189cec7cc5cd422204
- https://kimyang-sun.tistory.com/entry/%EB%A6%AC%EC%95%A1%ED%8A%B8-%EB%A6%AC%EB%8D%95%EC%8A%A4-%ED%88%B4%ED%82%B7%EC%9C%BC%EB%A1%9C-%EC%83%81%ED%83%9C%EA%B4%80%EB%A6%AC-React-Redux-Toolkit
- http://blog.hwahae.co.kr/all/tech/tech-tech/6946/
-
