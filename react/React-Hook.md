###### 2019.06.07
## 1. Introducing Hooks
React v 16.8에 추가된 기능으로써 클래스를 작성하지 않고 state나 다른 리액트의 특성을 사용하게 해주는 기능입니다.
Hook은 function으로, React state와 lifeCycle을 function componenets로 hook Into(연결) 해줍니다.
Hook은 class안에서 사용할수 없고, class가 아닌 React component를 사용하게 합니다.
## 2. Hooks
### 1) useState

```js
import React, { useState } from 'react';

function Example() {
  // Declare a new state variable, which we'll call "count"
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

```useState```는 Hook의 한 종류로, function component에 local state를 추가합니다.
React는 리랜더링 사이에서 이 state를 보존합니다.

```useState```는 getter, setter 와 비슷한 한 쌍의 값들을 리턴합니다.

useState는 파라미터로 하나의 인자를 받는데, 이는 state의 initial value가 됩니다.

### 2) Effective Hook
이전에 data fetching, subscriptions, 또는 React componenet의 DOM을 수동적으로 변화시킨 적이 있을겁니다. 이러한 리렌더링은 다른 구성요소에 영향을 미칠 수 있고, 렌더링 중에 수행할 수 없기 때문에 ```Side Effect```라고 부릅니다.
```js
import React, { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  // Similar to componentDidMount and componentDidUpdate:
  useEffect(() => {
    // Update the document title using the browser API
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```
```useEffect```를 호출하면, React에게 DOM 변화가 flushing된 이후 effect를 실행하라는 것 입니다.
Effects는 componenet안에서 선언되기 때문에 props와 state에 접근 할 수 있습니다.
기본값으로 React는 매 렌더 이후 effect를 실행하게 됩니다.

## 3. 유의 사항
* 훅은 <b>top level</b>에서 정의해야 합니다. hook을 조건문, 반복문, 중첩된 함수에서 호출하면 안됩니다.
* 훅을 <b>React function componenets</b>에서만 정의 및 사용 해야 합니다. 일반 자바스크립트 함수에서 사용하면 안됩니다.