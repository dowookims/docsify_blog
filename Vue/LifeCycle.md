# Vue LifeCycle
###### 2019.10.14

Vue-cli를 활용하여 작업을 할 때, 렌더링을 위해 서버에 필요한 data를 API 요청을 통해 가져오는 작업을 합니다. 저는 주로, 맨처음 화면 로드시 필요한 데이터를 가져오기 위해 `app.vue` 에서 `created` 또는 `mounted`, 그 중에서도 `mounted` 단계에서 서버로 API 콜 요청을 많이 보냅니다. 그 이유는, 리액트에서 `axios`로 요청을 보낼 때 `componentDidMount`에서 작업을 많이했기 때문입니다. 그러나 단지 이 논리만으로는 제가 `mounted` lifecycle method를 쓰는데 충분한 이유가 되지 않기에, 제대로 이해하기 위해 다시한번 라이프 사이클에 대해 정리를 해봅니다.

![라이프사이클](./_assets/lifecycle.png)

뷰 인스턴스가 생성되고 나면, 일련의 라이프 사이클 메서드가 실행이 됩니다. data observation, template compiling, mount the instance to the DOM, update DOM when data change occurs 등등 다양한 기능등을 담당하고, 이 기능들을 담당하는 함수인 `beforeCreate`, `create`, `mounted` 등을 `lifecycle hooks`라고 합니다. `React hooks`! 도 생각이 나네요.

!> 모든 `lifecycle hooks`들은 this context를 가지고 있기 때문에 절대 arrow function으로 호출해서는 안됩니다.
