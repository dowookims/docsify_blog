## Basic Components
##### 1st updated 2019-07-25
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

## Practice
공식 문서에 있는 내용을 기반으로 실습을 진행해 봤습니다.
> npx create-react-app react-router-practice

```
src
-- components
---- App.js
-- pages
-- index.js
-- index.css
```
위처럼 리액트 프로젝트를 생성 한 후, `App.js`를 `components` 디렉토리로 이동하고 작업을 진행합니다.

```js
import React from 'react'

const App = () => {
  return (
    <div>
      Hello Router
    </div>
  )
}

export default App
```
현재 프로젝트에서 `react-router-dom`을 사용하기 위해 설치를 합니다.
```bash
> yarn add react-router-dom
```
설치가 완료되면 `react-router-dom`을 `App.js`에 적용합니다.
```js
import React from 'react'

// BrowserRouter 를 Router라는 별칭으로 사용합니다.
import { BrowserRouter as Router } from 'react-router-dom'
const App = () => {
  return (
    <Router>
      <div>
        Hello Router
      </div>
    </Router>
  )
}

export default App
```
이제 `react-router-dom`에 있는 content들을 사용할 환경을 간단하게 구축해 봤습니다. 이제 실제 사용할 메뉴바와, 메뉴를 클릭 할 때 나타날 component들을 만들겠습니다.

* Menu.js

```js
// src/components/Menu.js
import React from 'react';
import { Link } from 'react-router-dom';

const Menu = () => {
  return (
    <div>
      <h2>안녕 나는 메뉴바</h2>
      <ul>
        <li><Link to="/">Home</Link></li>
        <li><Link to="/about">About</Link></li>
        <li><Link to="/about/react">About React</Link></li>
        <li><Link to="/posts">Posts</Link></li>
      </ul>
    </div>
  )
}

export default Menu;
```

링크에 부여된 url을 통해 Routing을 할 때 보여줄 component들은 `pages` 디렉토리에 작성합니다.
```js
// src/pages/Home.js
import React from 'react';

const Home = () => {
  return (
    <div>
      <h2>Home!</h2>
      <p>안녕나는집</p>
    </div>
  )
}

export default Home;
```

```js
// src/pages/About.js
import React from 'react';

const About = () => {
  return (
    <div>
      <h2>About!</h2>
      <p>안녕나는소개</p>
    </div>
  )
}

export default About
```

```js
// src/pages/Post.js
import React from 'react';

const Post = () => {
  return (
    <div>
      <h3>Post에용</h3>
      <p>나는 몇번째 Post에용</p>
    </div>
  )
}

export default Post;
```

이렇게 Pages에 들어갈 컴포넌트들을 만들고, 이를 `index.js`에서 묶어줍니다.

```js
// src/pages/index.js
export { default as Home }  from './Home';
export { default as About } from './About';
export { default as Post } from './Post';
export { default as Posts } from './Posts';
```
이렇게 묶어주는 이유는 나중에 다른 파일에서 작업시 `pages`내에 있는 파일들을 `import` 할 때 객체로 한번에 불러오게 하기 위함입니다.

이제 `App.js`에 넣어줍니다.

```js
// src/components/app.js
import React from 'react'
import Menu from './Menu'

/* src/pages/index 에서 작업을 해주지 않았으면 
각 컴포넌트들을 하나하나 import 해야 합니다.
*/
import { Home, About, Post } from '../pages'

// BrowserRouter 를 Router라는 별칭으로 사용합니다.
import { BrowserRouter as Router } from 'react-router-dom'
const App = () => {
  return (
    <Router>
      <Menu />
      {/* exact를 하지 않으면 '/'이 들어가는 모든 url에 Home Component가 렌더링 됩니다. */}
      <Route exact path="/" component={Home} />
      <Route path="/about/:name?" component={About} />
      <Route path="/posts/" component={Post} />
    </Router>
  )
}

export default App
```

간단한 react-router-dom 프로젝트가 완성되었습니다.

## Detail
여기서 조금 더 들어가자면, Route에서 적합한 `path`에 맞게 component가 렌더링 되는데, 그 element는 Props로 `history`, `location`, `match`를 받습니다.

```js
import React from 'react';

const Post = ({ history, location, match}) => {
  console.log(`history : ${history}`)
  console.log(`location : ${location}`)
  console.log(`match : ${match}`)
  return (
    <div>
      <h3>Post에용</h3>
      <p>나는 몇번째 Post에용</p>
    </div>
  )
}

export default Post;
```
이 객체들을 활용하여 query 값과 history를 이용한 이동, 현재 location의 값 등을 파악 할 수 있습니다.

### 1) history
현재 라우터를 조작할 때 사용합니다. `history.goBack()`, `history.block()` 등으로 다양한 작업을 진행 할 수 있습니다.
`history`는 `push`와 `replace`함수를 가지고 있는데, `push`는 이동시 페이지 방문 기록을 남기고, `replace`는 남기지 않습니다.

`action`은 현재 `history`의 상태를 알려주는데, 처음 방문했을 때 `POP`, 링크를 통한 라우팅 또는 `push`를 통한 라우팅을 했을 떄 `PUSH`, replace를 하면 `REPLACE` 가 나타납니다.

### 2) location
location은 현재 페이지의 주소 상태를 알려줍니다. 이 location 값은 어떤 라우트 컴포넌트에서 조회하든 같은데, search 값에서 url query를 읽을때 사용하거나, 주소가 바뀐것을 감지 할 때 사용합니다.

```js
// /user/3
{
  "pathanme": "/user/3",
  "search": "",
  "hash": "",
  "key": "xmsczi"
}
```

```js
componentDidMount(prevProps, prevState){
  if(prevProps.location !== this.props.location){
    // url 주소가 바뀌었을때 처리
  }
}
```

```js
//src/pages/About.js
import React from 'react';
import queryString from 'query-string' //외장 라이브러리라 설치 필요

const About = ({ history, location, match }) => {
  const query = queryString.parse(location.search) {/* 이렇게 하면 url에 있는 query값이 객체로 반환됩니다. */}
  
  return(
    (...)
  )
}
```

### 4) withRouter
Route를 통해 렌더링 되는 컴포넌트는 `props`로 `history`, `match`, `location`을 받지만 그렇지 않은 컴포넌트는 받지 못합니다. 이런 안타까운 친구들을 위해 react-router-dom은 `withRouter`를 만들었습니다. 사용방법은 아주 간단합니다.

```js
// src/componetns/Menu.js
import React from 'react';
import { Link } from 'react-router-dom';

const Menu = () => {
  return (
    <div>
      <h2>안녕 나는 메뉴바</h2>
      <ul>
        <li><Link to="/">Home</Link></li>
        <li><Link to="/about">About</Link></li>
        <li><Link to="/about/react">About React</Link></li>
        <li><Link to="/posts">Posts</Link></li>
      </ul>
    </div>
  )
}

export default Menu;
```
이 귀염둥이 메뉴에서

```js
// src/componetns/Menu.js
import React from 'react';
// Add withRouter
import { withRouter, Link } from 'react-router-dom';

const Menu = ({ history, location, match }) => {
  return (
    <div>
      <h2>안녕 나는 메뉴바</h2>
      <ul>
        <li><Link to="/">Home</Link></li>
        <li><Link to="/about">About</Link></li>
        <li><Link to="/about/react">About React</Link></li>
        <li><Link to="/posts">Posts</Link></li>
      </ul>
    </div>
  )
}

// edit this
export default withRouter(Menu);
```
`withRouter`를 불러와서, export 해주는 곳에 컴포넌트를 withRouter 함수에 인자로 넣어주면 끝!
