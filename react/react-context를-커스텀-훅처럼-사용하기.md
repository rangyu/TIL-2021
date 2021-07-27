# React Context를 커스텀 훅처럼 사용하기

React Context를 커스텀 훅처럼 사용하는 방법에 대해 알아보았다. 


- 참고 : <a href="https://fatmali.medium.com/use-context-and-custom-hooks-to-share-user-state-across-your-react-app-ad7476baaf32" target="_blank">https://fatmali.medium.com/use-context-and-custom-hooks-to-share-user-state-across-your-react-app-ad7476baaf32</a>

## Context API 

기본적으로 React Context API를 사용하는 방법은 다음과 같다.

~~~JSX
const MyContext = React.createContext(defaultValue);
~~~

React.createContext로 생성한 context에는 Provider와 Consumer가 담겨져 있다. value를 가지고 있는 상위 컴포넌트는 Provider를 이용해서 Context의 변경되는 정보들을 하위 컴포넌트들에게 전달할 수 있다.

~~~JSX
<MyContext.Provider value={/* some value */}>
  ...
</MyContext.Provider>
~~~

하위 컴포넌트들은 Consumer를 통해 context 내 변경되는 값들을 전달 받는다.

```JSX
<MyContext.Consumer>
{(value) => (/* render something based on the context value */)}
</MyContext.Consumer>
```

## useContext

Consumer를 사용하는 대신 useConext 훅을 사용하면 하위 컴포넌트를 Consumer로 감싸지 않고도 context 값을 가져올 수 있다. 
```JSX
const value = React.useContext(MyContext)
```

## useContext를 커스텀 훅처럼 사용하기

useContext를 사용해서 커스텀 훅처럼 사용하면 보다 편리하게 context를 이용할 수 있다. 아래의 예제는 앱 정보를 기록하는 AppContext를 만들고 이를 이용한 AppProvider와 useAppContext를 만드는 방법이다.

### AppContext.js
먼저 context를 모아두는 contexts 폴더를 만든 뒤, AppContext.js 파일을 생성한다.AppContext.js 내에 AppProvider와 useAppContext를 다음과 같이 작성한다.

```JSX
import React, { useState } from "react";

const AppContext = React.createContext(null);

export const AppProvider = ({ app: initialApp, children }) => {
  const [app, setApp] = useState(initialApp);

  return (
    <AppContext.Provider value={{ app, setApp }}>
      {children}
    </AppContext.Provider>
  )
}

export const useAppContext = () => React.useContext(AppContext);
```

### AppProvider 사용하기
```JSX
import React from "react";
import { Routes, Home, Posts } from "components";
import { AppProvider } from "contexts/AppContext";

const app = {
  name: "MyApp",
  pages:[
    { path: "/", title: "Home", component:Home },
    { path: "/posts", title: "Posts", component:Posts },
  ],
};

const App = () => {
  return (
    <AppProvider app={app}>    
      <Routes/>
    </AppProvider>
  )
}

export default App;
```

### useAppContext 사용하기

이제 만약 하위 컴포넌트에서 AppContext 값을 사용하고 싶다면, useAppContext를 사용하면 된다.

```JSX
import React from "react";
import { useAppContext } from "contexts/AppContext";
...
const Routes = () => {
  const { app } = useAppContext();
  return (
    <BrowserRouter>
      <ScrollTop/>
      <Switch>
      {app.pages.map((page, i) => (
        <Route key={`route-${i}`} page={page}/>
      ))} 
      </Switch>
    </BrowserRouter>
  )
}

export default Routes;
```
