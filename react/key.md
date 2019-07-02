아래와 같은 코드가 있는데
```js
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <li>{number}</li>
  );
  return (
    <ul>{listItems}</ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

이렇게 리스트를 컴포넌트 안에서 렌더링 하면, 코드는 동작하고 dom에 `ul`과 `li`는 추가가 됩니다. 그러나 console에서 경고창이 나타나는데, 이 경고는 `key`를 추가하라고 이야기 합니다. 왜 `key`가 필요할까요?

React에서 사용하는 jsx 문법에서 key는 React가 어떤 아이템이 바뀌고, 추가되고, 삭제되었는지 도움을 줍니다.  
쉽게 말해서 `object(객체)`에서 어떤 특정 값을 찾을 때 이 객체를 순회하며 원하는 요소를 찾을 때 걸리는 복잡도는 객체 내부의 아이템의 개수를 n이라 가정했을 때 O(N)이 됩니다.

저는 주로 axios로 api를 호출해서 나온 결과값을 `componentDidMount`에서 렌더링 해줄 때 많이 사용하는데, 이 때 마땅히 넣을 key 값이 생각나지 않아 for 문을 사용 할 때 index 값을 key로 넣어줍니다.  
코드로 보면

```js
const todoItems = todos.map((todo, index) =>
  <li key={index}>
    {todo.text}
  </li>
);
```
이렇게 사용합니다.  
그러나 리액트 공식문서에서는 아이템의 순서가 바뀔 수 있는 경우 key에 인덱스를 사용하지 않는게 좋다고 합니다. 이로 인해 성능이 저하되거나 컴포넌트 state에 따른 문제가 발생 할 수 있기 때문입니다.

이 한 구절이 제가 이 글을 작성하게 된 이유가 되었습니다. 우리는 무언가를 쓸 때 왜, 어떻게 이런 주의가 나타나는지 알아두면 이후 개발시 큰 도움이 될 수도 있기 때문이죠.

React  공식문서에 [Reconciliation(재조정)](https://reactjs-kr.firebaseapp.com/docs/reconciliation.html) 에 내용이 잘 나와있습니다. 저는 이를 조금 더 풀어서 제가 이해한 방향으로 적어보겠습니다.

리액트를 사용하다보면 `render()`라는 함수를 자주 보게 됩니다. render()이후  jsx로 dom을 조작하는 로직을 작성하기 때문에 이 함수가 React 요소 트리를 생성하는 것으로 생각 할 수도 있으나 이는 잘못된 생각이라고 합니다.

render()함수는 `state`나 `props` 업데이트 이후 다른 React tree를 반환하고, 이전의 트리 구조를 render()로 반환한 트리로 변환하기 위한 최소한의 연산을 수행하여 UI를 효율적으로 업데이트를 하려고 합니다. 이를 구현한 알고리즘은 O(n^3)의 복잡도를 가지며(n 은 트리 내 요소 갯수), 이는 생각보다 비싼 알고리즘입니다.

React 개발자들은 두 가지 가정에 의하여 복잡도를 O(N)으로 만들 수 있는 알고리즘을 사용하는데
1. 다른 타입의 두 요소는 서로 다른 트리를 생성하며
2. 개발자는 `key prop`을 이용해 다른 렌더링 사이에서 안정적인(변화하지 않은) 자식 요소에 대한 힌트를 얻을 수 있으며

실제로 이런 가정은 모든 실제 사용 사례에 유효하다고 합니다.

이를 더 구체적으로 이야기 하자면,

1. 알고리즘은 다른 컴포넌트 타입의 서브트리를 일치시키려고 하지않습니다. 아주 유사한 출력을 가진 두 컴포넌트 타입을 번갈아 사용하는 경우 같은 타입으로 만들 수 있습니다. 실제로 이런 이슈를 발견하지 못했습니다.

2. 키는 안정적이고 예측가능하며 고유해야합니다. `Math.random()` 을 이용해 생성된 키와 같은 불안정한 키는 많은 컴포넌트 인스턴스와 DOM 노드가 불필요하게 다시 작성되어 하위 컴포넌트의 성능 저하 및 state 손실을 초래할 수 있습니다.

## 1. 다른 타입의 요소
루트 요소가 다른 타입을 가질 때 마다 React는 이전 트리를 해체하고 새로운 트리를 처음부터 빌드합니다. 즉 `<a>`에서 `<img>`, `<Article>`에서 `<Comment>` 등 다른 두 요소는 완전한 재 빌드로 이끌게 됩니다.

트리를 해체할 때 이전 DOM 노드는 제거되며. 컴포넌트 인스턴스는 `componentWillUnmount()` 를 받습니다. 새 트리가 만들어질 때 새 DOM 노드는 DOM에 들어갑니다. 컴포넌트 인스턴스가 `componentWillMount()` 를 받은 후 `componentDidMount()` 를 받습니다. 이전 트리와 관련한 모든 state가 손실됩니다.

루트 아래의 모든 컴포넌트의 마운트도 해제되고 state가 파괴됩니다. 
```js
<div>
  <Counter />
</div>

<span>
  <Counter />
</span>
```
이 때 이전 `Counter`를 제거하고 새로운 `Counter`를 마운트 합니다.

## 2. 같은 타입의 DOM요소
같은 타입의 두 React DOM 요소를 비교할 때 React는 두 요소의 속성을 보고 같은 DOM 노드를 유지하며 변경된 속성만 업데이트합니다.

### 2-1. 같은 타입의 Component 요소
컴포넌트가 업데이트될 때 인스턴스가 동일하게 유지되면 렌더링간에 `state`가 유지됩니다. React는 새 요소와 일치하도록 컴포넌트 인스턴스의 `props`를 업데이트하고 인스턴스에서 `componentWillReceiveProps()` 와 `componentWillUpdate()` 를 호출합니다.

다음으로 render() 메서드가 호출되고 비교 알고리즘은 이전 결과와 새로운 결과에 반복됩니다.

### 2-2 children에서 반복
기본적으로 DOM 노드의 자식 노드에 대해 반복할 때 React는 동시에 두 자식 노드 목록을 반복하여 실행하고 차이가 있을 때마다 변형을 생성합니다.

예를들어
```js
// before
<ul>
  <li>first</li>
  <li>second</li>
</ul>

// after
<ul>
  <li>first</li>
  <li>second</li>
  <li>third</li>
</ul>
```
이 경우에는 잘 동작합니다. React의 `<li>first</li>`와 `<li>second</li>`는 일치하기에 트리에 `<li>three</li>`를 추가합니다.

그러나 다음과 같은 예시는 제대로 작동하지 않습니다.
```js
<ul>
  <li>Duke</li>
  <li>Villanova</li>
</ul>

<ul>
  <li>Connecticut</li>
  <li>Duke</li>
  <li>Villanova</li>
</ul>
```

이 경우 React는 모든 자식을 변형시켜 `<li>Duke</li>` 및 `<li>Villanova</li>` Subtree를 유지 할 수 있음을 인지하지 못합니다.

이런 이슈를 해결하기 위해 React는 `key`를 지원합니다.  
자식이 키를 가질 때 React는 오리지널 트리의 자식을 후속 트리의 자식과 비교할 때 키를 사용합니다.  

예를 들어 위의 비효율적인 예제에 key 를 추가하면 트리를 효율적으로 변환할 수 있습니다.
```js
<ul>
  <li key="2015">Duke</li>
  <li key="2016">Villanova</li>
</ul>

<ul>
  <li key="2014">Connecticut</li>
  <li key="2015">Duke</li>
  <li key="2016">Villanova</li>
</ul>
```
이제 React는 `'2014'` 키를 가진 요소가 새로운 것임을 알고 있고 `'2015'` 와 `'2016'` 키를 가진 요소는 옮겨진 것임을 압니다.

여기서 `key`는 글로벌하게 고유할 필요는 없고 형제 사이에서만 고유하면 됩니다. 그리고 아까, 글을 쓰게 된 이유인 배열의 아이템 인덱스를 키로써 전달하는걸 조심해야 하는 이유는

재정렬하지 않은 아이템에서는 잘 동작하지만 재정렬할 때는 느리게 동작하기 때문입니다. 즉 위의 예제와 같은 경우 재정렬이 필요하기 때문에 key를 배열의 인덱스로 사용할 때 주의 해야 합니다.

또한 인덱스를 키로써 사용하면 재정렬을 통해 컴포넌트 state에 문제가 발생할 수도 있습니다. 컴포넌트 인스턴스는 키에 따라 업데이트되고 재사용됩니다. 만약 키가 인덱스라면 아이템을 이동하면 해당 아이템이 변경됩니다. 그 결과로 제어되는 input 같은 컴포넌트 state가 예기치 않은 방식으로 혼합되고 업데이트될 수 있습니다.

이게 `key`를 사용하는 이유이면서 동시에 배열의 인덱스를  `key`로 사용 할 때 주의해야 할 점 입니다.