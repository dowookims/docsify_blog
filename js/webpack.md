###### 2019-07-25

## 1. intro
제가 프론트엔드 공부를 하면서 가장 어려운 난이도를 가지고 있는 삼대장이 있습니다. `redux` , `SSR` 그리고 `webpack과 babel`

Webpack을 처음 만난 것은 18년 10월 이었습니다. React Component 에 Styling하기 위해 CRA를 eject하고 webpack config에 있는 sass-loader에 option을 줘야 할 일이 있었습니다. 

책으로 React를 공부하던 때고, 그 책에서 사용하는 CRA 는 major version이 1이 었는데, 2로 바뀌면서 웹팩 설정이 바뀐걸 모르고 몇개월동안 삽질 하던게 생각이 나네요.

그때 충격과 트라우마 때문인지, webpack과 babel의 존재에 대해 보고 생각만 해도 식은 땀이 납니다. 지금 웹팩과 바벨을 공부를 하고, 어느정도 세팅은 되지만 cra를 eject 했을때 babel config에 대한 충격은 아직도 각인이 되어서 언제나 어려운 존재로 저에게 인식 되고 있습니다.

## n. Code Splitting
### 1) Vendor 설정

프로젝트 전역에서 사용하는 라이브러리를 분리 합니다. react, react-dom, redux, styled-component등을 분리하는 거죠.
code splitting의 대상은 개발자가 직접 작성한 코드보다, 외부 라이브러리들을 `vendor`로 따로 분리합니다. 이렇게 하면 추후 프로젝트를 업데이트 할 때 업데이트 하는 파일 크기를 최소화 할 수 있다고 합니다.

### 2) Dynamic import
   
import 를 함수로 사용하면, Promise 를 반환합니다. import() 함수는 아직 표준은 아니지만 stage-3 단계에 있는 dynamic import 라는 문법입니다. 현재는, webpack 에서 지원해주고 있는 함수이기에 프로젝트에서 별도의 설정 없이 바로 사용 할 수 있습니다. 이 함수는 모듈을 비동기적으로 CommonJS 형태로 불러오니, 따로 default 를 명시해주어야 합니다. 위에서 사용한 코드에서는 "default 를 notify 를 부르겠다" 라고 설정을 해주었습니다. [velopert-velog](https://velog.io/@velopert/react-code-splitting)
```js
import React, { Component } from 'react';

class App extends Component {
  handleClick = () => {
    import('./notify')
      .then(({ default: notify }) => {
        notify();
      });
  };
  render() {
    return (
      <div>
        <button onClick={this.handleClick}>Click Me</button>
      </div>
    );
  }
}

export default App;
```

### 3) Component Code splitting (Post by [velopert](https://velog.io/@velopert/react-code-splitting))
```js
import React from 'react';

const SplitMe = () => {
  return <div>날, 필요할때만 불러.</div>;
};

export default SplitMe;
```
```js
import React, { Component } from 'react';

class App extends Component {
  state = {
    SplitMe: null
  };
  handleClick = () => {
    import('./SplitMe').then(({ default: SplitMe }) => {
      this.setState({
        SplitMe
      });
    });
  };
  render() {
    const { SplitMe } = this.state;
    return (
      <div>
        <button onClick={this.handleClick}>Click Me</button>
        {SplitMe && <SplitMe />}
      </div>
    );
  }
}

export default App;
```

### 4) Code Splitting by Hoc
```js
import React, { Component } from 'react';

const withSplitting = getComponent => {
  // 여기서 getComponent 는 () => import('./SplitMe') 의 형태로 함수가 전달되야합니다.
  class WithSplitting extends Component {
    state = {
      Splitted: null
    };

    constructor(props) {
      super(props);
      getComponent().then(({ default: Splitted }) => {
        this.setState({
          Splitted
        });
      });
    }

    render() {
      const { Splitted } = this.state;
      if (!Splitted) {
        return null;
      }
      return <Splitted {...this.props} />;
    }
  }

  return WithSplitting;
};

export default withSplitting;
```

```js
import React, { Component } from 'react';
import withSplitting from './withSplitting';

const SplitMe = withSplitting(() => import('./SplitMe'));

class App extends Component {
  state = {
    visible: false
  };
  handleClick = () => {
    this.setState({
      visible: true
    });
  };
  render() {
    const { visible } = this.state;
    return (
      <div>
        <button onClick={this.handleClick}>Click Me</button>
        {visible && <SplitMe />}
      </div>
    );
  }
}

export default App;
```