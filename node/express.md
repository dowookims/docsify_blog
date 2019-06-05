## 1. [Express](https://expressjs.com/)란?

```
Express is a minimal and flexible Node.js web application framework that 
provides a robust set of features for web and mobile applications.

Express는 웹 및 모바일 애플리케이션을 위한 강력한 기능 셋을 제공하는 
유연하고 경량화된 Node.js 웹 애플리케이션 프레임워크 입니다.
```

## 2. 설치
```bash
mkdir express_test
cd express_test
npm init
npm install express --save
```

## 3. 사용법

```js
// express_test/app.js
const express = require('express')
const app = express()
const port = 3000

app.get('/', (req, res) => res.send('Hello World!'))

app.listen(port, () => console.log(`Example app listening on port ${port}!`))
```
커맨드에서 `node app.js`을 실행.

이렇게 세팅하면 사용자가 A부터 Z까지 빌드해야 하기 때문에 만약 자동적으로 더 내용들을 추가해주기를 바란다면
`Express application generator`를 사용합니다.
```bash
npm install express-generator -g
```
`express-generator 를 전역설치(-g)하면 이 이후부터 express 프로젝트를 만들때
```bash
express [Projectname]
```
으로 설정할 수 있으며,
```bash
express --view=pug myapp
```
으로 view engine 도 설정 할 수 있습니다.

## 4. Routing
Routing refers to determining how an application responds to a client request to a particular endpoint, which is a URI (or path) and a specific HTTP request method (GET, POST, and so on).

라우팅(Routing)은 응용 프로그램이 특정 엔드포인트에 대한 클라이언트 요청(URI(또는 경로)와 특정 HTTP 요청 방법(GET, POST 등))에 어떻게 대응하는지 결정하는 것을 말합니다. 

각각의 라우트는 하나 이상의 handler function을 가지고 있으며, 각 라우터는 URI가 매칭이 될 때 실행됩니다.

라우트의 default 구조는
```js
app.METHOD(PATH, HANDLER)
```
로 구성이 되는데 이는
* app은 express의 인스턴스로 app.js를 구성 할 때 `const app = express()`에서의 `app`을 의미합니다.
*  METHOD는 HTTP request method로 소문자로 사용된다(get, post, patch, delete, put)
*  PATH는 서버의 path이며
*  Handler는 route가 일치할 때 실행되어야 할 함수입니다.

```js
app.get('/', function (req, res) {
  res.send('Hello World!')
})

app.post('/', function (req, res) {
  res.send('Got a POST request')
})

app.put('/user', function (req, res) {
  res.send('Got a PUT request at /user')
})

app.delete('/user', function (req, res) {
  res.send('Got a DELETE request at /user')
})
```

## 5. Service static files
이미지, CSS 또는  JS파일을 serve하기 위해서 `express.static`이라는 빌트인 미들웨어 함수를 사용해야 합니다.
`express.static(root, [options])`
예를들어, `public`이라는 폴더에 CSS, JS, 이미지들이 들어가 있고 이걸 serve 하려고 하면
```js
app.use(express.static('public'))
```
을 사용하면 됩니다. 

!> Express는 정적 디렉토리와 관련된 파일을 검색하므로 정적 디레토리의 이름은 URL의 일부가 아닙니다.
즉, 위에서 `public`에 정적 파일을 serve 하면 웹 상에서 정적 파일들의 위치는
```
http://localhost:3000/images/kitten.jpg
http://localhost:3000/css/style.css
http://localhost:3000/js/app.js
http://localhost:3000/images/bg.png
http://localhost:3000/hello.html
```
이렇게 구성이 됩니다.

만약, 정적 폴더를 여러개를 두려고 하면
```js
app.use(express.static('public'))
app.use(express.static('files'))
```
이렇게 구성 할 수 있으며, 만약 가상 경로 prefix를 설정하여 정적 파일들을 serve 하려면
```js
app.use('/wow', express.static('public'))
```
이렇게 구성할 수 있습니다. 이는 `public`이라는 폴더 내부의 정적 파일들을 `wow`라는 가상 경로 prefix를 사용하며 serve 한다는 것이며 이는
```
http://localhost:3000/wow/images/kitten.jpg
http://localhost:3000/wow/css/style.css
http://localhost:3000/wow/js/app.js
http://localhost:3000/wow/images/bg.png
http://localhost:3000/wow/hello.html
```
이렇게 웹 상에서 serve 됩니다.

추가적으로, 우리가 정적 파일이 들어가있는 폴더의 경로를 저장할때
```js
app.use(express.static('public'))
```
이라고 했지만, 절대 경로로 정적 폴더의 위치를 명시하는게 좋기 때문에
```js
app.use('/static', express.static(path.join(__dirname, 'public')))
```
이렇게 사용하는게 Best practice입니다.