# export default 오류
<img width="700" alt="오류1" src="https://user-images.githubusercontent.com/97326130/176637144-8df36711-9450-4aee-9e2c-c0fc764c4cbe.png">

### 문제 상황
- 갑자기 ```yarn dev```로 프로젝트를 실행하자 오류 발생
```typescript
import React from 'react'

export const index = () => {
  return (
    <div> index </div>
  )
}
```

### 해결
- ```Next.js``` 에서는 ```export default```를 사용해야 한다고 한다 .. *(허무함)*
```typescript
import React from 'react'

const index = () => {
  return (
    <div>index</div>
  )
}

export default index;
```

### 참고
- https://stackoverflow.com/questions/59873698/the-default-export-is-not-a-react-component-in-page-nextjs
