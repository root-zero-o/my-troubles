# theme.colors 오류
<img width="700" alt="error" src="https://user-images.githubusercontent.com/97326130/176862964-432c8748-c609-4813-9c0f-66b7d475d07e.png">

### 문제 상황
- ```tailwind CSS```를 이용해 ```layer```를 만들던 도중 문제 발생
- ```styles/elements.css```
```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  h1 {
    @apply font-serif font-semibold text-3xl text-white;
  }
  h2 {
    @apply font-serif font-medium text-lg text-white;
  }
}

@layer components {
  .container {
    @apply bg-ivory bg-green rounded-xl p-28 grid text-center space-y-5;
  }
}
```
- ```tailwind.config.js```
```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  purge: ["./pages/**/*.{js,ts,jsx,tsx}", "./components/**/*.{js,ts,jsx,tsx}"],
  theme: {
    screens: {
      "mobile" : {"min" : "320px", "max" : "480px"},
      "tablet" : {"min" : "768px", "max" : "1024px"},
      "desktop" : {"min" : "1024px"}
    },
    colors: {
      ivory: "#FCF8E8",
      green: "#94B49F",
      orange: "#ECB390",
      red: "DF7861",
    },
    extend: {},
  },
  plugins: [],
};

```

### 해결
- ```tailwind.config.js```에 ```theme.colors```를 지정하면, 원래 있던 색들이 모두 리셋된다 !
- 따라서 사용할 색들을 모두 ```theme.colors```에 넣어주어야 함 !
- ```tailwind.config.js```
```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  purge: ["./pages/**/*.{js,ts,jsx,tsx}", "./components/**/*.{js,ts,jsx,tsx}"],
  theme: {
    screens: {
      "mobile" : {"min" : "320px", "max" : "480px"},
      "tablet" : {"min" : "768px", "max" : "1024px"},
      "desktop" : {"min" : "1024px"}
    },
    colors: {
      ivory: "#FCF8E8",
      green: "#94B49F",
      orange: "#ECB390",
      red: "DF7861",
      white: "#FFFFFF"  // 사용할 색을 추가해주었다.
    },
    extend: {},
  },
  plugins: [],
};

```

- white를 따로 추가해주자 오류가 사라졌다. 해결!🙆‍♀️

### 참고
- https://stackoverflow.com/questions/67098510/class-text-white-with-tailwind-does-not-work
