###### 2019-05-30

styled-component와 styled-reset 으로 React 내부에서 CSS Styling을 합니다.

## 1. Making GlobalStyle 

styled-component로 global sytle을 먼저 잡아서 진행합니다.
> CRA에서 바로 진행을 합니다.
```txt
src
-- Components
---- App.js
-- styles
---- GlobalStyles.js
```

```react
// src/Styles/GlobalStyles.js
import { createdGlobalStyle} from "styled-components"
import reset from "styled"

export default createdGlobalStyle`
  ${reset}
  * {
    box-sizing:border-box;
  }
`

```

이렇게 초기 작업을 하는 이유는 styled-component를 사용하기전 전역 css 를 초기화 하기 위해서입니다.

```react
// src/Components/App.js
import React from "react"
import GlobalStyles from "../Styles/GlobalStyles"

export default () => <>
  <GlobalStyles /> 
  Hello
</>

```
이후 `yarn start`로 리액트 서버를 열게 되면 `<title>` 태그 밑에 `css reset` 코드가 들어간 것을 볼 수 있습니다.


## 2. Using ThemeProvider
위에서 만든 GlobalStyle에 styled-component의 ThemeProvider를 추가합니다.

  *ThemeProvider* 란 `styled-components`에서 제공하는 Full Theming을 지원하는 `wrapper content` 입니다.

  이 컴포넌트는 모든 리액트 컴포넌트에 context API를 통해 Theme을 제공합니다.

!> ThemeProvider는 렌더링 할 때 자식 요소들을 리턴합니다.

그전에 우리가 자주 사용할 속성들에 대해서 파악한 후, 이를 Theme.js에 저장합니다.

```react
// src/Styles/Theme.js

const BORDER_RADIUS="4px"
const BOX_BORDER="1px solid #e6e6e6"

export default {
  bgColor: "#FAFAFA",
  blackColor: "#262626",
  darkGrayColor: "#999",
  lightGrayColor: "#c7c7c7",
  redColor: "#ED4956",
  blueColor: "#3897f0",
  darkBlueColor: "#003569",
  borderRadius: "4px",
  boxBorder: "1px solid #e6e6e6",
  whiteBox: `${BORDER_RADIUS};
              ${BOX_BORDER};
              background-color:white;
            `
};
```

App.js에 들어가서 작업을 더 합니다.
```react
// src/App.js
import React from 'react';
import { ThemeProvider } from "styled-components";
import GlobalStyles from "../Styles/GlobayStyles";
import Theme from '../Styles/Theme';

export default () => (
  <ThemeProvider theme={Theme}>
    <GlobalStyles />
  </ThemeProvider>
)

```
이렇게 기본적인 Theming 구성을 하고 리액트 컴포넌트에 스타일링을 할 때 `theme`을 추가해서 작업을 진행하게 됩니다.


## toastify 
Notification과 관련된 것

