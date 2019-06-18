## 1. Basic Components
react-router에는 
* router components
* route matching components
* navigation components

세가지 타입의 컴포넌트가 있습니다.
```js
import React from "react";
import { BrowserRouter as Router, Route, Link } from "react-router-dom";

function Index() {
  return <h2>Home</h2>;
}

function About() {
  return <h2>About</h2>;
}

function Users() {
  return <h2>Users</h2>;
}

function AppRouter() {
  return (
    <Router>
      <div>
        <nav>
          <ul>
            <li>
              <Link to="/">Home</Link>
            </li>
            <li>
              <Link to="/about/">About</Link>
            </li>
            <li>
              <Link to="/users/">Users</Link>
            </li>
          </ul>
        </nav>

        <Route path="/" exact component={Index} />
        <Route path="/about/" component={About} />
        <Route path="/users/" component={Users} />
      </div>
    </Router>
  );
}

export default AppRouter;
```
### 1) Routers

모든 react router 어플리케이션의 코어는 router 컴포넌트가 되어야 합니다.

웹프로젝트를 위해 react-router-dom은 `<BrowserRouter>`와 `<HashRouter>`를 제공합니다.

이 두가지 router 컴포넌트는 특별한 `history` 객체를 만들어서 반환해줍니다.

일반적으로 말해서 서버에서 리퀘스트에 대한 응답을 하는 서버를 가지고 있다면 `<BrowserRouter>`를 쓰는게 좋고 정적 파일 서버를 사용한다면 `<HashRouter>`를 사용하는게 좋습니다.


### 2) Route Matching
Route matching components는 `<Route>`와 `<Switch>` 두개가 있습니다.

Route matching은 `<Route>`의 path prop과 현재 location의 pathname을 비교하며 행해집니다.

`<Route>`가 매치되면, 이에 포함되는 콘텐츠를 렌더링하고 그렇지 않을 경우 null을 렌더링 합니다.

!> path가 없는 <Route>는 모든 것에 매치 됩니다.

`<Route>`는 location에 기반해서 콘텐츠가 어디에 렌더링하고 싶은지에 따라 원하는 곳 어디에서든 포함 될 수 있습니다.

가능한 <Route>를 서로 옆에 나열하는 것이 로직상 편리할 것입니다. `<Switch>`컴포넌트는 `<Route>`의 그룹과 함께 사용 됩니다.

`<Switch>`는 반드시 <Route>를 그룹화 하는데 필요한 것은 아닙니다. 그러나 이렇게 사용하는 것은 꽤나 유용합니다.
`<Switch>`는 자식 요소인 <Route>를 iterate하면서 현재 location에서 가장 첫번째로 매치되는 <Route>의 콘텐츠를 렌더링 합니다. 이는 같은 경로를 가진 다른 라우터들 중 우선되는 것을 렌더링 하거나 매치되지 않는 현재 location을 표현할 때 404 page를 보여주는 등에 활용 될 수 있습니다.

### 3) Route Rendering Props
우리는 어떻게 컴포넌트를 렌더링 할지에 대해 선택하는 <Route>에서 주어지는 세가지 props를 선택 할 수 있습니다. 이는
* component
* render
* children
입니다.

