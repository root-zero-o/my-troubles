# 이미지 import 오류

<img width="800" alt="ts1" src="https://user-images.githubusercontent.com/97326130/176121184-5eacff3c-a902-44a1-be53-e01a8f5c357c.png">

## 문제상황
- ```src/images/home```에서 이미지를 import하자 오류 발생

## 해결
1. ```src```폴더 안에 ```custom.d.ts``` 파일을 만든다.
2. ```custom.d.ts```파일 안에 다음과 같이 작성한다.
```typescript
declare module "*.jpg";  // 사용할 확장자명을 적어준다.
declare module "*.png";
declare module "*.jpeg";
declare module "*.gif";
```
3. 문제 해결 !🙆‍♀️

## 참고
- https://www.carlrippon.com/using-images-react-typescript-with-webpack5/
