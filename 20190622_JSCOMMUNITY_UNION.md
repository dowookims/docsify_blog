#  JAVASCRIPT IS EVERYWHERE

###### 2019.06.22

## S1  CRA + SSR을 구현하며 배운 점 | 승형수

###### (feat. TS) 승형수 20@kross.kr |[승형수](https://github.com/hunskyhoochu)

eject를 하지 않고 ssr를 써보자! 여기서 직면한 문제들, 여기서 해결 해야 할 문제들에 대해서.

### 1.  SERVER-SIDE-RENDERING ?

기존 MVC 패턴에서는 데이터를 정의하고, 파싱할지 규칙을 짜서 HTML template 문법을 통해 한 가지 페이지에 접속을 하게 된다면 그 페이지에 대한 모든 HTML을 웹 프레임워크에서 다 짜주고 통으로 클라이언트에게 넘겨준다(traditional)



But 이런게 점점 버거워지기 시작하는데, 수많은 사람들이 서버에 몰리면 서버에 브라우징 할 때마다 이 연산을 서버가 스스로 마쳐서 보내줘야 하는데, 서버 자원은 한정되는데 유저 요청을 다 처리한다는게 트래픽적으로 부담이 된다. 그러나 요새 컴퓨터가 다 좋아졌기 때문에 이 부담을 클라이언트에게 부담을 하겠다 해서 나온게 `CSR`  이다.



```
백화점에서 완제품을 사오는 것? SSR
```



CSR은 최소한의 html을 그리고, 화면 전체 렌더링을 위한 JS를 서버에서 클라이언트에게 보내준다. 이러한 JS는 사용자의 컴퓨터에서 처리하게 되는데, 이렇게 하면 서버의 경우 진입할때 커넥션만 열어주면 되기 때문에 서버에서 수많은 요청에 대한 부담이 줄어들게 된다.

```
이케아에서 조립형 소파를 사와서 사용자가 조립한다? => CSR
```



그러나 여기서 문제가 생기는게, 사용자 같은 경우는 문제가 없으나 웹 크롤러가 페이지 정보를 긁어 갈 때 아무 내용도 없는 빈 페이지이기 때문에 아무것도 가져갈 수 없어서 SEO  문제가 생긴다. 그리고 모든 데이터들을 클라이언트 컴퓨터에서 처리하기 때문에 로딩이 느리고, 사용자 컴퓨터에 따른 문제에 대한 예측을 하기가 어렵게 된다.



그래서 SSR은 초기 렌더링을 마친  HTML과 일부 업데이트를 위한 JS를 보낸다. 즉, 서버에서 기본적인 구성이 되기 때문에 검색엔진이 대응하기에 좋다.

```
위에서 한번, 장에서 한번
```

서버에서 간단한 렌더링을 한번 하고, 클라이언트에서 한번 더 함으로써 더 효율적으로 작용하게 하는게 SSR



### 2. Implementation

서버에서 한번

1) ReactDOMServer.renderToString() 함수로 react app을 html string 으로 변환

2) 서버의 '^/$' route에서 출력하게 한다.

클라이언트에서 또 한번

3) 'build/index.html'에는 react 런타임 코드와 페이지 코드가 붙어있음!

4) 모든 리소스를 static으로 서버에 제공하자.

```react
// src/html.text
export default ReactDOMServer.renderToString(<App />);
```

```tsx
// server/htmlRenderer.tsx

export default (html: string, htmlPath: string) => (req:express.Request, res: express.Response) => {
  fs.readFile(htmlPath. 'utf8'), (err,data) => {
    
  }
}
```

```tsx
// server/index.tsx
```



`data-reactroot` 

render | hydrate(render: 전체 HTML 렌더링  | hydrate: 기존 HTML과 비교 후 부분 업데이트)

```react
ReactDOM.hydrate(<App />, document.getElementById('root'));
```



componontDidMount까지는 서버 렌더링이 된다. 그러나 functioncal Component의 hook의 경우 아직 이부분이 지원되지 않기 때문에 클래스형 Component를 사용하는게 좋당

### 3. CHECK-LIST

스파게티는 먹을 때에만 : 서버코드를 분리하자

asset-manifest.json | build 안에 담긴 보물 , 빌드 안에 있는 파일에 대한 패스를 json으로 줌으로 초기 rendering시 유용하게 쓸 수 있다.

직선보다 빠른 굴절 | TypeScript를 사용하는 편이 좋은 이유: 성능 또는 개발에서 더용이하다.

#### 1) 스파게티는 먹을 때에만

`renderToString()` 함수가 서버 코드와 섞이기 마련이다.  

그래서 `bootstrap.js` 파일을 추가로 작성해 각자의 코드를 따로 불러올 것.  express 객체와 renderToString()의 결과물을 bootstrap.js에서 가져와 app.listen()을 하는게 더 나을지도 모른다.

`.git submodule` 운용 가능 : SSR SERVER를 따로 만들어 git repo내부의 도 다르 repo 참조 파일을 통해 관리 👩‍💼 👨‍💼!

이 부분은 coupling 코드를 막기 위해서

그래서 결국은

Asset-manifest.json : build폴더에 생성된 모든 자산의 일람표. html string을 만들 때 static file의 참조를 가져다 쓸 수 있다. 

{Logo} => 를 상대주소로 입력쓰 해준다. 이 부분은 나중에 따로 더 자세히 볼 것.

### 2) 직선보다 빠른 굴절

왜 TS가  SSR에 도움이 될까? Tsc 컴파일을 통해. CommonJS 파일 생성 가능 => babel을 쓰지 않아 성능 향상:rocket:

거기에 타입 체킹으로 안전한 설계 가능.

### 4. Extra

필수 패키지 설치 이슈

* Redux

  서버 렌더링 단계에서 데이터를 미리 주입하고 싶을 때만 사용. index.html에 인라인으로 window 전역  store객체 삽입 -> 클라이언트  코드가 읽어들임

  <script></script>

* React-router-dom: 

* React-Loadable: 



_성공보다 빠른 시행착오를 응원합니다_



##S2  나의 JS  사용기 | 최종욱



##S3 Nuxt로 

1. Single page application (nuxt build —spa) 
2. statically generated (nuxt generate)
3. server side render(nuxt build) => ec2, lambda 필요(node로) serverless

### Serverless framework

Build web mobile and Not applications with serverless architectures using AWS Lambda, Azure Functions, Google CloudFunctions & more

AWS Lambda 로 CloudFormation을 통해 동작한다.

서비스를 만들때 소스코드만 작성하면 되는데 RDS나 키네틱스등을 서버리스 프레임워크에 사용 할 수 있다.

서버리스 프레임워크로 서비스 별 마이크로 아키텍처 관리, 유지 가능

Serverless.yaml를 작성한다. 인터넷 참고.

serverless에서 HTTP event는 API Gateway를 사용한다. 보통은 API를 만들 때 사용한다.

그런데 왜 Serverless에 nuxt를 올려야 하는가?

serverless 를 사용하는 장점으로써

* 과금은 사용한 만큼만(나노 써도 한달에 만원 정도 나옴) => 많이 써도 확장 할 필요 없이 쓴만큼만 내면된다.
* Auto Scale(이걸 자동으로 해준다.)
* 무중단 배포: Serverless는 배포하면 바로 반영한다(로드 밸런스에 대한 고려를 할 필요가 없어진다.)

Nuxt Serverless 구현도 생각보다 쉽다. API 서버 만드는 것처럼

[github](GitHub.com/wan2land/js2019-serverless-nuxt)

Serverless.yaml 작성해야 한다.

Nuxt도 express를 쓰고, 이걸 lambda에 넣을 수 있다.



데이터 저장용으로는 S3를 사용한다(저장 할 때만 금액 청구됨)

람다의 소스코드 최대업로드 가능한 용량은 250mb 대부분의 용량은 이미지 + 폰트라서 이 부분은 s3로 저장하는게 낫다.

Nuxt의 경우 배포 할 때 클라이언트 / 서버가 나뉘기 때문에 클라이언트 관련 소스만 올리면 된다.

정적파일 처리: 요청받으면 정해진 파일을 던져주는데 이를 매끄럽게 처리해야 할 필요가 있다.

* 그이유로 비용절감,  용량절감, 무중단 배포가 가능하다.

일반 웹사이트 배포를 할 때 일반 웹사이트에 들어가면 HTML을 불러오고 이 안에 있는 css, js를 가지고 온다. 그리고 배포가 된다. 브라우저에 캐싱이 되는데, 사용자가 새로운 페이지로 이동 시 html 파일을 가지고오고 js, css를 가지고 온다.

SSR FRAMEWORK에서는 페이지 이동시 자바스크립트 등 필요한 리소스만 가져오게 되는데 첫번째 화면을 보고 있는 상태에서 배포가 될 경우 원래 있던 파일들은 다 사라지고 그 다음 버전이 나타나게 되는데 이 와중에 사용자가 페이지를 이동하면 url을 이전에 것을 가지고 있기 때문에 원하는 요청 파일도 없어지고, 파일이 없다는 에러가 나오면서 사용자가 이용 사항에 문제가 생긴다. 물론 사용자가 새로 고침을 하면 문제가 없지만, 이런것도 처리해 줘야 하기 때문에, 정적 파일을 처리하면 이런 문제들이 해결된다고 한다.



즉, s3를 cdn처럼 사용해서 static file로 가져오는게 편하당.



1. Serverless.yml 파일에 cloudFormation을 이용하여. s3 bucket을 사용한다.
2. nuxt.config.js 파일에 publicPath 옵션에 s3 bucket URL 지정한다.
3. nuxt build 한것을 s3에 올려야 하는데

수동으로 업로드 할 수도 있는데 이를 자동화 하는 방법을 고민해 보자. 그래서 찾아본게 Serverless plugin.

[서버리스 플러그인](Serverless.com/blog/writing-serverless-plugins)

배포할 때 나오는 내용들을 알아야한다.

서버리스 라이프 사이클

* Cleanup / initialize : 처음 에만 나오는데 임시파일 삭제, 등등
* setUpProviderConfiguration : initialize 과정에서 생성한 
* createDeploymentArtifacts: 우리가 작성한 소스코드를 업로드 할 수 있도록 빌드한다.
* compileFunctions, compileEvents: 이런저런거
* deploy => createStack, checkForChanges, uploadArtifacts, validateTemplate, updateStack

플러그인이 해야 할 일 : Serverless배포 전 Nuxt를 자동으로 빌드하고 빌드 후 생성된 정적파일을 버전별로 s3에 업로드 한다.

플러그인 HookPoint: createDeploymentArtifacts. 훅을 이 라이프 사이클 직전에 넣어준다. deploy이후에 파일을 업로드 해야 한다. 클라우드 포메이션을 통해서 s3 버킷을 만들었기 때문에 deploy이후 적응이 된다.

`github 참고`

플러그인을 적용해본다.

`github 참고`

람다와s3의 경우 리전 종속되기 때문에 lambda@edge와 등등을 고려해서  cdn처리를 해보아야 한다.



$ https

Nginx, Apache 등 사용하면 http에서https로 리다이렉션을 쉽게 설정할 수 있으나 serverless 는 api gateway를 사용하고, 이는 https만을 지원한다. 사용자가 명시적으로 https를 언급해줘야 한다. nuxt는 프론트엔드 페이지를 올리는데 이를 강제할 수가 없다. 그래서 이게 약점아닌 약점처럼 됬었는데,  lambda에서 application loadbalancer에서 이 event를 지원하고 저번 주 수요일에 가능하게 되었당.





요약!

1. nuxt 의 her 장점 그대로 개발하장
2. nuxt serverless로 lambda위에 운영해서 비용절감
3. serverless nuxt 플러그인으로 쉽게 적용 가능
4. 글로벌 서비스 쉽게 할 수 있당





##S4 React Hooks + TS + functional = 아름다움 | 유윤재

###### CEO of DNext Inc. block chain based service with RN = Alice(Cross Platform)

dynamic typing => null point error etc…. => how can make stable SW?

1. use Functional Components | 16.8
2. TypeScript | Compile tyime type check
3. React Hooks | Code Reuse seems like making react as new



### 1. Functional Component

Old source code => class based code

* make member variables, methods render( use JSX like html)
* use lifeCycle method



functional component => simple

use arrow function. no need to use complex things.

return 키워드를 생략하고 더욱 간결하게 작성할 수도 있다.



주의해야 할 사항으로 함수 내의 로직은 렌더링이 수행될 때 마다 매번 계산된다 => useCompute

* 무거운 계산하는 내용을 이 functional component내에 넣는것은 별로 안좋다. 

* 함수 내에서 새로운 함수를 만들면 더더욱 오버헤드가 크다.
* 함수형 컴포넌트에서는 this.props를 가져 올 수 없다. 함수의 인자로 받아야 한다. 왜냐하면 this는 글로벌 객체로 잡혀있기 때문에(this는 클래스에서 사용, 그 외에 사용시 window라 undefined가 뜬다.)



### 2. ReactHooks

#### 1) useState

hook이란 리액트에서 지원해주는 함수의 집합. 훅이 나온 이유는 functional component가 존재했고, dumb component가 존재했고 이 친구는 상위 컴포넌트에서 보내주는 props만 사용했으나, functional programming이 유행하면서 어떻게 functional component에도 로직을 사용할 수 있을까 고민하다 나온게 React hook

```
import React, { useState} from 'react';
const MyComponent = ({ name }) => {
  const [ count, setCount ] = useState(0);
  return (
  	<View>
      <Button onpress={{this.onPress }}>
        <Text>Count: { count }</Text>
      </Button>
    </View>
  )
}
```

동적으로 변하는 카운트 값을 사용 할 수 있다.

useState에 값이 아닌 함수를 전달 할 수 있는데 예를 들어 아까 나온 무거운 작업을 통해 초기값을 설정해야 할 경우에는

```react
const MyComponent = ({ name }) => {
  const [ count, setCount ] = useState(() => intenseCalculation());
  등등
}
```

이렇게 하면 count가 set될때 한번 함수를 실행하고 이 이후 call 할때 함수가 다시 실행되지 않는다.

또한 state의 대상이 되는 값은 배열, 객체도 될 수 있다.

**모든 훅은 함수의 최상위 레벨에서 호출되어야 하므로 함수 안에서 호출될 수 없다**

그 이유는 훅이 호출되면 내부적으로 복잡한 과정으로 state를 관리하는데, 함수가 호출되는 순서를 기록하고, 여러개의 훅이 있다면 다 기록하고 다음번 렌더링이 됬을 때 훅이 실행되면 인덱스를 기준으로 캐싱되어 있는 상태와 비교해서 가져오기 때문에 그 순서가 흐트러지면 안되므로 반드시 훅함수는 어떤일이 있더라도 전부다 최상위에 기록 되어야 한다.



#### 2) useCallback()

호출될 함수, 재 계산을 위한 의존성 목록을 인자로 받는다.

```react

import React, { useState, useCallback } from 'react';
const MyComponent = ({ name }) => {
  const [ count, setCount ] = useState(0);
  const onPress = useCallback(() => setCount(count +1 ), [count])
  return (
  	<View>
      <Button onpress={{ onPress }}>
        <Text>Count: { count }</Text>
      </Button>
    </View>
  )
}
```

useCallback은 뒤에 있는 디펜던시 내용이 달라질 때만 다시 callBack을 호출하여 값을 계산된다. 만약 뒤에 인자가 없으면 처음 렌더링시만 계산하고 그 이후에 계산이 되지 않는다. 그래서 계산할 때 마다 계속 1로 세팅되기 때문에 반드시 dependency에 무언가를 넣어 주어야 한다. 배열을 사용하는 이유는 위를 참고 할것.



async함수도 지원된다

```react
const onPress = useCallback(async() => {
  await funcAsync();
}, []);
```

useCallback 사용시 두번째 인자에 빈 배열을 전달하면 콜백 함수는 최초 상태로 고정되고 이후에 재계산 되지 않는다. 이벤트가 핸들링 되지 않을 때 dependency를 참고할 것



#### 3) useEffect

lifeCyclemethod는 함수형 컴포넌트에 사용 할 수 없는데, 이 친구를 통해 사용 할 수 있게 되었읍니다.

useEffect()는 호출될 함수, 재계산을 위한 의존성 목록을 인자로 받는다. 함수는 렌더링이 완료된 후에 수행된다.

이는 렌더링 된 이후 useEffect를 사용한다. 호출되고 사용하는게 아님.

```react

import React, { useState, useCallback, useEffect } from 'react';
const MyComponent = ({ name }) => {
  const [ count, setCount ] = useState(0);
  const onPress = useCallback(() => setCount(count +1 ), [count]);
  useEffect(() => {
    alert(name);
    return () => {
      alrt("clean - up")
    }
  }, [name]);
  return (
  	<View>
      <Button onpress={{ onPress }}>
        <Text>Count: { count }</Text>
      </Button>
    </View>
  )
}
```

useEffect는 정리 함수(clean-up function)를 리턴할 수 있다.이는 componentWillUnmount()에 해당한다.

useEffect에서 전달하는 두번째 인자의 값들이 변경될 때마다 함수가 수행된다. 이는 컴포넌트의 componentDidMount()에 해당한다.

여러개의 dependency를 추가 할 수 있음.

useEffect의 두번째 인자는 전달하지 않아도 되지만 그럴 경우 함수는 렌더링이 수행되는 프레임마다 호출된다.(퍼포먼스 이슈)

빈배열을 호출하면 한번만 호출하지만, 아무것도 주지 않으면, 계속 렌더링을 한다. 작성하다 느리면. useEffect에 의존성이 전달되었는지 확인 할 것

useEffect에서는 async 함수를 전달 할 수 없다. 그러나 함수 내부에서 정의해서 호출해서 사용하면 된다. 직접적인 전달이 불가능 한것



#### 4)  customHook

여러개의 훅을 묶어서 나만의 Hook을 만들 수 있다.

여러가지 로직을 묶어서 커스텀 훅을 만들 수 있다. 클래스 컴포넌트로는 구현할 수 없는 함수형 컴포넌트만의 장점



```react
const MyComponent = (P name ) => {
  const { count, onPress } = useIncrementer(name);
  return (
  <View>
    	<Button onPress={onPress}>
        <Text>{ count }</Text>
      </Button>
  </View>

      )    
}

const useIncrement = (name: string) => {
  const [ count, setCount ] = useState(0);
  const onPress = useCallback(() => setCount(count + 1), [count]);
  useEffect(() => {
    alert(name);
  }, []);
  return { count, onPress};
}
```



### TS

Runtime 에 type checking 하는 propstypes가 불편하다. 컴파일 시 가능하고 싶다! => ts!

ts를 하면 더 명시적이고

```tsx
interface MyComponentProps {
  name?: string;
  age?: number;
  female: boolean;
}

const MyComponent: React.FunctionComponent<MyComponentProps> = ({ name, age, female})
=> {
  return (
      <View>
  	<Text>Name: {name}</Text>  
    <Text>age: {age}</Text>
   	<Text>female: {female}</Text>  
  </View>)
  )
}

//또는

const MyComponent = ({ name, age, female }: MyComponentProps) => {
  return (
  <View>
  	<Text>Name: {name}</Text>  
    <Text>age: {age}</Text>
   	<Text>female: {female}</Text>  
  </View>)
}

```

? 있으면 string. 또는 undefined. ? 없으면 반드시 지정된 타입만.



```tsx
const [ count, setCount ] = useState(0); //인자의 값을 추론하는 기능이 있음
const [ count, setCount ] = useState<number>(0); // 위에것과 이거는 반드시 null만 가능
const [ count, setCount ] = useState<number | null>(0);// count type은 number or null
```

```tsx
const onPress = useCallback((text) => alert(text), [text]);
const onPress = useCallback((text: string) => alert(text), [text]);
```