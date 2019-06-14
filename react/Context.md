###### 6.10

> Context provides a way to pass data through the component tree without having to pass props down manually at every level.

리액트 공식문서에 나와있는 문장으로, `Context`는 모든 레벨에서 props down으로 데이터를 전달할 필요 없이, 컴포넌트 트리를 통해 데이터를 전달하는 방법을 제공 한다고 합니다.
기존 리액트 앱에서는 `Redux`를 사용하지 않을때 자식 노드에 있는 특정 데이터를 다른 자식 노드에 전달하려고 한다면 공통 부모를 가진 노드에 까지 데이터를 올리고, 그 올린 데이터를 다시 밑으로 내려주는 복잡한 방식을 채택하게 됩니다. 이는 어플리케이션이 복잡해질 수록 수정해야 할 파일들이 많아 질 뿐만 아니라 로직을 상당히 복잡하게 만들어버리기 때문에, 이를 해결하기 위한 방법으로 나타난   API라고 볼 수 있습니다.

## 1. 언제 `Context`를 사용해야 할까?
`Context`는 현재 인증된 사용자 또는 테마 등 React component tree에서 `글로벌`로 간주할 수 있는 데이터를 공유하도록 설계 되었습니다. 즉, 전역 변수처럼 사용해야 할 데이터가 있거나, 컴포넌트가 다른 컴포넌트에 데이터를 사용 해야 하는데, 그 데이터를 공유하는 로직이 복잡해지는 경우에 사용하는게 좋습니다.

## 2. API
### 1) React.createContext
```js
const MyContext = React.createComtext(defaultValue);
```
`Context` 객체를 생성합니다. React가 이 Context 객체를 구독하고 있는 컴포넌트를 렌더링 할 때, 트리에서 가장 가까운, 일치하는(Matching) Provider로부터 현재 Context value를 읽습니다.

`defaultValue` 인자는 매치되는 Provider가 없을 때에만 사용됩니다.

### 2) Context.Provider
```js
<MyContext.Provider value={/* some value */}>
```
`Provider`를 이용하여 사용되는 React Component들은 Context를 구독 또는 변경 할 수 있습니다.

### 3) Context.Consumer
```js
<MyContext.Consumer>
  {value => /* render something based on the context value */}
</MyContext.Consumer>
```
`Consumer`는 `context` 변경을 구독하는 리액트 컴포넌트 입니다. `Consumer`를 통해 함수형 컴포넌트 안에서 `Context`를 구독 할 수 있게 합니다.

이를 활용하면 다음과 같은 코드를 사용할 수 있습니다(벨로퍼트님 블로그에서 사용된 코드 입니다.)

!> 아래 소스 코드는 velopert님 블로그(https://velopert.com/3606)에 기재된 코드임을 밝힙니다.
```js
// src/contexts/sample.js
import React, { Component, createContext } from 'react';

const Context = createContext();

const { Provider, Consumer: SampleConsumer } = Context; 

class SampleProvider extends Component {
  state = {
    value: '기본값입니다'
  }
  actions = {
    setValue: (value) => {
      this.setState({value});
    }
  }

  render() {
    const { state, actions } = this;
    const value = { state, actions };
    return (
      <Provider value={value}>
        {this.props.children}
      </Provider>
    )
  }
}

export {
  SampleProvider,
  SampleConsumer,
};
```
`src/app.js`
```js
import React from 'react';
import LeftPane from './components/LeftPane';
import RightPane from './components/RightPane';
import { SampleProvider } from './contexts/sample';

const App = () => {
  return (
    <SampleProvider>
      <div className="panes">
        <LeftPane />
        <RightPane />
      </div>
    </SampleProvider>
  );
};

export default App;
```

`src/componenets/Receives.js`
```js
import React from 'react';
import { SampleConsumer } from '../contexts/sample';

const Receives = () => {
  return (
    <SampleConsumer>
      {
        (sample) => (
          <div>
            현재 설정된 값: { sample.state.value }
          </div>
        )
      }
    </SampleConsumer>
  );
};

export default Receives;
```


## 참고
[velopert.log](https://velopert.com/3606)

[Context - react](https://reactjs.org/docs/context.html#before-you-use-context)