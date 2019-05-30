*Apollo Boost*는 Apollo Client를 시작하는 가장 쉬운 방법이다.

`yarn add apollo-boost`로 설치를 해주고 바로 작업에 들어가겠습니다.

> CRA로 작업 후 src 폴더에 Apollo 폴더를 만든 후  Client.js 라는 파일을 만들어 줍니다.

!> 선행되어야 할 작업으로 backend에 graphql 서버가 있어야하고, 이 서버가 구동되어 있어야 합니다.

```react
// src/Apollo/Client.js
import ApolloClient from "apollo-boost"

export default new ApolloClient({
  uri:"http://localhost:4000" // 자신의 graphql URI
});
```

이후 `src`에 있는 자신의 App.js 에 들어가서 `ApolloProvider`를 추가합니다. 이를 위해서 `yarn add react-apollo-hooks`를 해야 합니다.

```react
import React from 'react';
import { ApolloProvider } from "react-apollo-hooks"
import { ThemeProvider } from "styled-components";
import GlobalStyles from "../Styles/GlobayStyles";
import Theme from '../Styles/Theme';
import AppRouter from './Router';
import Client from '../Apollo/Client';

// ApolloProvider에서 필수로 필요한 인자는 client 입니다.

export default () => (
  <ThemeProvider theme={Theme}>
    <ApolloProvider clinet={Client}>
      <GlobalStyles />
      <AppRouter isLoggedIn={false}/>
    </ApolloProvider>
  </ThemeProvider>
)

```

또는 `index.js`에 `ApolloProvider`를 넣어서 진행 할 수도 있습니다.
```react
//  index.js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './Components/App';
import { ApolloProvider } from "react-apollo-hooks"
import Client from './Apollo/Client';

ReactDOM.render(
<ApolloProvider client={Client}>
  <App />
</ApolloProvider>
, document.getElementById('root'));
```
!> 이렇게 진행할 경우 App에 있는 provider를 제거 해야 합니다.

`ApolloProvider`를 작성한 이후
`src/Apollo`에 `LocalState`를 작성합니다. `LocalState`란 기본적으로 Client에 없는 State를 말합니다.

지금 만들고 있는 앱에서는 로그인 유무를 확인하기 위한 LocalState를 만들 예정입니다. 그래서 다음과 같이 작성하였습니다.
```react
//src/Apollo/LocalState.js

export const defaults = {
  isLoggedIn: localStorage.getItem("TOKEN") !== null ? true : false
};

export const resolvers = {
  Mutation: {

    logUserIn: (_, { token }, { cache }) => {
      localStorage.setItem("TOKEN", token);
      cache.writeData({data: {
        isLoggedIn: true
      }
    });
    return null;
    },

    logUserOut: (_, __, { cache }) => {
      localStorage.removeItem("TOKEN");
      window.location.reload();
      return null;
    }
  }
};
```
이후 `Client.js`에서 수정된 사항을 추가 해 줍니다.
```react
// src/Apollo/Client.js

import ApolloClient from "apollo-boost"
import { defaults, resolvers } from "./LocalState";
export default new ApolloClient({
  uri:"http://localhost:4000",
  clientState: {
    defaults,
    resolvers
  }
});
```