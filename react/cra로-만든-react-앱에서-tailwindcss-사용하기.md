# CRA(create-react-app)로 만든 React 앱에서 tailwindcss 사용하기 (+ JIT)

CRA(create-react-app)로 만든 리액트 앱에서 tailwindcss를 사용하는 방법을 알아보자. (+ JIT)

- 참고 : https://dev.to/akov/how-to-set-up-tailwindcss-with-create-react-app-jit-feature-1m20

## tailwindcss 설치하기

먼저 npm 혹은 yarn으로 tailwindcss 관련 필수 패키지들을 설치한다. tailwindcss는 개발 시에만 사용되고 배포시에서는 일반적인 css로 변환된 파일을 쓰기 때문에 dev 모드로 설치한다. npm에서는 `-D`, yarn에서는 `--dev` 옵션을 붙이면 개발용으로 설치된다. 최신 패키지를 설치하기 위해 `@latest`도 붙여주자.

- tailwindcss
- postcss
- postcss-cli
- autoprefixer

### npm 
```
npm install -D tailwindcss@latest postcss@latest postcss-cli@latest autoprefixer@latest
```

### yarn 
```
yarn add --dev tailwindcss@latest postcss@latest postcss-cli@latest autoprefixer@latest
```

## tailwind.conifg.js

```
npx tailwindcss init
```
위와같은 명령어로 프로젝트 내 루트에 `tailwind.conifg.js`를 생성할 수 있다. 실행하면 아래와 같은 기본 설정 파일이 생성된다.

```js
// tailwind.conifg.js
module.exports = {
  purge: [],
  darkMode: false, // or 'media' or 'class'
  theme: {
    extend: {},
  },
  variants: {
    extend: {},
  },
  plugins: [],
}
```

tailwindcss의 JIT(Just In Time) 모드를 활성화하면 tailwindcss의 스타일이 적용될때 걸리는 딜레이를 줄일 수 있다. `tailwind.conifg.js`를 다음과 같이 수정해서 JIT 모드를 사용하자.

```js
// tailwind.config.js
module.exports = {
  mode: "jit",
  purge: ["./src/**/*.{js,jsx,ts,tsx}", "./public/index.html"],
  darkMode: false, // or 'media' or 'class'
  theme: {
    extend: {},
  },
  variants: {
    extend: {},
  },
  plugins: [],
}
```

## src/tailwind.css

프로젝트의 src 폴더 안에 `tailwind.css` 파일을 만들고 다음과 같이 작성한다.

```css
/* tailwind.css */
@tailwind base;
@tailwind components;
@tailwind utilities;
```

## postcss.config.js

프로젝트의 루트에 `postcss.config.js` 파일을 생성하고 다음과 작성한다.

```js
// postcss.config.js
module.exports = {
  plugins: { 
    tailwindcss: {}, 
    autoprefixer: {} 
  }
};
```

## 파일 변경 시 자동으로 tailwindcss 빌드하기 

이제 js 파일이 변경될 때 마다 자동으로 tailwindcss 빌드하도록 해주자.이를 위해서 특정 확장자의 파일들을 watch 하는 chokidar-cli과 동시에 스크립트를 실행시켜주는 concurrently를 설치한다.

### npm
```
npm install -D chokidar-cli concurrently
```

### yarn
```
yarn add --dev chokidar-cli concurrently
```

## package.json

마지막으로 package.json 내의 `"scripts"`부분을 다음과 같이 바꿔준다. 

### npm
```
...
"scripts": {
  "start": "react-scripts start",
  "build": "npm run watch:css && react-scripts build",
  "test": "react-scripts test",
  "eject": "react-scripts eject",
  "watch:css": "postcss src/tailwind.css -o src/index.css",
  "watch": "chokidar \"./src/**/*.js\" -c \"npm run watch:css\"",
  "dev": "concurrently \"npm run watch\" \"npm run start\""
},
...
```

### yarn
```
...
"scripts": {
  "start": "react-scripts start",
  "build": "yarn watch:css && react-scripts build",
  "test": "react-scripts test",
  "eject": "react-scripts eject",
  "watch:css": "postcss src/tailwind.css -o src/index.css",
  "watch": "chokidar \"./src/**/*.js\" -c \"npm run watch:css\"",
  "dev": "concurrently \"yarn watch\" \"yarn start\""
},
...
```

이제 `npm run dev` 혹은 `yarn dev` 명령어로 실행하면 react 앱에서 tailwindcss를 JIM 모드로 사용할 수 있다.


