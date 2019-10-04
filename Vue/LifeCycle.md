# Vue LifeCycle
###### 2019.10.14

Vue-cli를 활용하여 작업을 할 때, 렌더링을 위해 서버에 필요한 data를 API 요청을 통해 가져오는 작업을 합니다. 저는 주로, 맨처음 화면 로드시 필요한 데이터를 가져오기 위해 `app.vue` 에서 `created` 또는 `mounted`, 그 중에서도 `mounted` 단계에서 서버로 API 콜 요청을 많이 보냅니다. 그 이유는, 리액트에서 `axios`로 요청을 보낼 때 `componentDidMount`에서 작업을 많이했기 때문입니다. 그러나 단지 이 논리만으로는 제가 `mounted` lifecycle method를 쓰는데 충분한 이유가 되지 않기에, 제대로 이해하기 위해 다시한번 라이프 사이클에 대해 정리를 해봅니다.

![라이프사이클](./_assets/lifecycle.png)

뷰 인스턴스가 생성되고 나면, 일련의 라이프 사이클 메서드가 실행이 됩니다. data observation, template compiling, mount the instance to the DOM, update DOM when data change occurs 등등 다양한 기능등을 담당하고, 이 기능들을 담당하는 함수인 `beforeCreate`, `create`, `mounted` 등을 `lifecycle hooks`라고 합니다. `React hooks`! 도 생각이 나네요.

!> 모든 `lifecycle hooks`들은 this context를 가지고 있기 때문에 절대 arrow function으로 호출해서는 안됩니다. (arrow function은 this를 가지고 있지 않기 때문에)
```
created: () => console.log(this.a)
vm.$watch('a', newValue => this.myMethod())
```

## 1) beforeCreate
* 모든 hook 중에서 가장 먼저 일어나는 함수.
* data와 event/ watcher가 setup 되고, instance가 initialize 된 후 동기적으로 즉시 호출된다.

## 2) created
* instance가 created된 이후 동기적으로 실행된다. 
* 이때 인스턴스는 data, computed properties, methods, watch 및 event에 대한 값들이 setup 되어 있기 때문에 이 값들을 활용한 라이프 사이클 메서드 활용이 가능하다. 
* 그러나mount stage에 도달하기 전이기 때문에, $el에 의한 접근은 불가능 하다.

## 3) beforeMount
* 이 hook은 SSR에서는 호출되지 않으며
* 첫 렌더링이 시작하기 전에 호출된다.

## 4) mounted
* instance가 mount되고 나서 호출된다. 
* el이 새로운 el instance에 의해 대체된다.
* mounted가 호출되었다고 해서, 자식 component들이 mounted 되었다고 보증할 수 없다. 
* 만약, 모든 component들이 mounted된 이후 작업을 진행하고 싶으면 `vm.$nextTick`을 mounted안에서 호출해야 한다.

```vue.js
mounted: function () {
  this.$nextTick(function () {
    // Code that will run only after the
    // entire view has been rendered
  })
}
```

## 5) beforeUpdate
* data가 변화되면, DOM에 patch되기 전에 실행되는 hook이다. 
* update되기 전 이미 DOM에 존재하는 el에 접근하기 좋은 hook이다(eventListener를 지우거나 할 때)
* SSR시 호출되지 않는다.

## 6) updated
* 가상 DOM에 의해 re-render가 발생한 이후 호출되는 hook이다. 
* DOM이 변화를 완료 했기 때문에, DOM에 종속된 작업을 할 때 적합한 hooks이다. 
* 여기서 state 값을 변경 하면 무한 루프에 빠질 수 있으니, state를 변경하는건 watch나 computed에서 처리를 해야한다. 
* mounted와 비슷하게 자식 component도 re-render된 것을 보장해 주지 않는다.
* SSR시 호출되지 않는다.

## 7) beforeDestory
* 이 hook은 뷰 인스턴스가 제거되기 직전에 호출된다. 
* 컴포넌트는 원래의 모습과 기능들을 가지고 있다. 
* SSR시 호출되지 않는다.

## 8) destroyed
* 이 hook은 vue instance가 제거 된 이후에 호출된다. 
* Vue 인스턴스의 모든 디렉티브가 바인딩 해제 되고 모든 이벤트 리스너가 제거되며 모든 하위 Vue 인스턴스도 삭제된다. 
* SSR시 호출되지 않는다.