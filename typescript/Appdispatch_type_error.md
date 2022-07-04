# AppDispatch type 오류

![error1](https://user-images.githubusercontent.com/97326130/177168353-4495926d-72d1-4593-9939-14f30419ef0e.png)

### 문제 상황
```javascript
// store/configStore.ts

import { configureStore, getDefaultMiddleware, MiddlewareArray } from '@reduxjs/toolkit';
import { createWrapper } from 'next-redux-wrapper';
import rootReducer from './modules';


  export const makeStore = () => configureStore({ 
    reducer : rootReducer,
    middleware: (getDefaultMiddleware) => getDefaultMiddleware()
  })

  export type AppStore = ReturnType<typeof makeStore>
  export type AppDispatch = typeof makeStore.dispatch; // 여기 dispatch에서 에러

const wrapper = createWrapper<AppStore>(makeStore)
  
export default wrapper;
```
- dispatch를 읽지 못하는 오류가 발생함

### 해결
```javascript
import { configureStore, getDefaultMiddleware, MiddlewareArray } from '@reduxjs/toolkit';
import { createWrapper } from 'next-redux-wrapper';
import rootReducer from './modules';


  export const makeStore = () => configureStore({ 
    reducer : rootReducer,
    middleware: (getDefaultMiddleware) => getDefaultMiddleware()
  })

  export type AppStore = ReturnType<typeof makeStore>
  export type AppDispatch = ReturnType<typeof makeStore>["dispatch"]  // 이렇게 바꿔주자 오류 해결

const wrapper = createWrapper<AppStore>(makeStore)
  
export default wrapper;
```

### 참고 
- https://stackoverflow.com/questions/61976204/how-to-get-the-appdispatch-typescript-type-in-redux-toolkit-when-store-is-initia
