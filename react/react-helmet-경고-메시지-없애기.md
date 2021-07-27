
# React Helmet 경고 메시지 없애기

## 문제

### 원본 코드
```
import Helmet from 'react-helmet';

...
<Helmet>
  <html lang={lang}/>
  <title>{title}</title>
  <meta name="description" content={desc} />
  <link rel="canonical" href="{url}"/>
  <body className={siteName}/>
</Helmet>
...
```

React Helmet을 사용했는데 콘솔창에 다음과 같은 경고 문구가 나타났다.

### Warning
```
index.js:1 Warning: Using UNSAFE_componentWillMount in strict mode is not recommended and may indicate bugs in your code. See https://reactjs.org/link/unsafe-component-lifecycles for details.

* Move code with side effects to componentDidMount, and set initial state in the constructor.

Please update the following components: SideEffect(NullComponent)
```

## 해결

React Helmet 대신 React Helmet Async로 교체하니 경고 문구가 사라졌다. :smile:

### 수정한 코드
```
import { Helmet, HelmetProvider } from 'react-helmet-async';

...
<HelmetProvider>
  <Helmet>
    <html lang={lang}/>
    <title>{title}</title>
    <meta name="description" content={desc} />
    <link rel="canonical" href="{url}"/>
    <body className={siteName}/>
  </Helmet>
</HelmetProvider>
...
```

- React-Helmet-Async : https://www.npmjs.com/package/react-helmet-async



