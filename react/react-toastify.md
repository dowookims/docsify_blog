토스트 메시지를 재빠르게 만들어주는 귀요미 라이브러리.

[React-toastify](https://github.com/fkhadra/react-toastify) 는 toast message를 쉽게 넣어주는 라이브러리 입니다.

## 1. 특성
* 정말 빨리, 그리고 쉽게 세팅할 수 있습니다. (익숙해지면 10초도 안걸립니다.)
* 커스터마이징 하기 쉽습니다.
* RTL 을 지원합니다.
* Swipe로 닫을 수 있습니다.
* toast 안에 React Component 를 넣을수도 있습니다.
* `onOpen`, `onClose` hook이 있어서 리액트로 다양한 작업을 진행할 수 있습니다.
* 등등등 다양한 기능들이 있습니다 😎

## 2. 사용법
> 시작을 위해 *react-toastify* 를 설치합니다.

```
> create-react-app toast

> cd toast

> yarn add react-toastify

```

```react
// src/app.js

  import React, { Component } from 'react';
  import { ToastContainer, toast } from 'react-toastify';
  import 'react-toastify/dist/ReactToastify.css';
  // minified version is also included


  class App extends Component {
    notify = () => toast("Wow so easy !");

    render(){
      return (
        <div>
          <button onClick={this.notify}>Notify !</button>
          <ToastContainer />
        </div>
      );
    }
  }
```
*toastify*의 특징은 **ToastContainer**가 *toast*들을 렌더하고, 다루게 됩니다. 스타크래프트의 캐리어와 인터셉터 같은 느낌이죠.
*toasts*들은 *ToastContainer*의 props를 상속받게 됩니다.

!> toastify 의 css도 toastify를 설치할 때 포함되기 때문에 css를 개인적으로 커스터마이징 해도 되지만 기본적으로 있는 것도 사용해도됩니다.
import 'react-toastify/dist/ReactToastify.min.css';

?> 루트 app 즉, src/app.js에 ToastContainer를 정의해 놓는게 *BEST PRACTICE* 입니다.

```react
 import React, { Component } from 'react';
  import { toast } from 'react-toastify';
  import 'react-toastify/dist/ReactToastify.css';

  // Call it once in your app. At the root of your app is the best place
  toast.configure()

  const App = () => {
    const notify = () => toast("Wow so easy !");
    
    return <button onClick={notify}>Notify !</button>;
  }
```
ToastContainer를 정의하지 않고 알아서 해주기를 원하신다면 `toast.configure()`을 선언해 주시면 됩니다. 그러면 Component들이  mount 될때 이 메서드가 mount를 해줍니다.

### 1. toast positioning
디폴트 값으로 toast message들은 우측 상단에 위치하게 됩니다. 
```react
// toast.POSITION.TOP_LEFT, toast.POSITION.TOP_RIGHT, toast.POSITION.TOP_CENTER
 // toast.POSITION.BOTTOM_LEFT,toast.POSITION.BOTTOM_RIGHT, toast.POSITION.BOTTOM_CENTER

  import React, { Component } from 'react';
  import { toast } from 'react-toastify';

  class Position extends Component {
    notify = () => {
      toast("Default Notification !");

      toast.success("Success Notification !", {
        position: toast.POSITION.TOP_CENTER
      });

      toast.error("Error Notification !", {
        position: toast.POSITION.TOP_LEFT
      });

      toast.warn("Warning Notification !", {
        position: toast.POSITION.BOTTOM_LEFT
      });

      toast.info("Info Notification !", {
        position: toast.POSITION.BOTTOM_CENTER
      });

      toast("Custom Style Notification with css class!", {
        position: toast.POSITION.BOTTOM_RIGHT,
        className: 'foo-bar'
      });
    };

    render(){
      return <button onClick={this.notify}>Notify</button>;
    }
  }
```
직관적으로 사용 할 수 있습니다.

### 2. Set autoclose delay or disable
* 디폴트 딜레이 시간을 지정 할 수 있습니다.
* autoClose라는 속성을 지정해 주면 됩니다.

```react
 import React from 'react';
  import { ToastContainer } from 'react-toastify';

  // close toast after 8 seconds
  const App = () => (
    <ToastContainer autoClose={8000} />
  );
```

조금 더 세부적으로 조절 할 수도 있습니다.
```react
  import React from 'react';
  import { ToastContainer, toast } from 'react-toastify';

  class App extends Component {
    closeAfter15 = () => toast("YOLO", { autoClose: 15000 });

    closeAfter7 = () => toast("7 Kingdoms", { autoClose: 7000 });

    render(){
      return (
        <div>
          <button onClick={this.closeAfter15}>Close after 15 seconds</button>
          <button onClick={this.closeAfter7}>Close after 7 seconds</button>
          <ToastContainer autoClose={8000} />
        </div>
      );
    }
  }
```

* autoClose의 값으로 false를 주면 자동으로 꺼지는걸 막을 수 있게되고, 사용자가 직접 끄게 만들게 합니다.

이 외에도 많은 기능들과 다양한 practice 가 있으니
[react-toastify](https://github.com/fkhadra/react-toastify)에서 확인 하면 되겠습니다.