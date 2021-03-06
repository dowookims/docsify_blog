###### 2019.06.17


## 1. XP란?
XP(eXtreme Programming)
* 애자일 개발 방법론의 일종으로, 1990년대 중반에 Kent Beck이 만듬.
* 모호하고 빠르게 변하는 요구사항을 가지고 일하는 중소규모 팀을 위한 경량화된 방법론
* 기본 개념은 여러 가지 효과가 좋은 실천 방법들을 극한으로 실천하는 것
* 전통적인 소프트웨어 개발 방법론과는 달리 문서를 강조하지 않고 변경을 장려한다. 즉, 돌아가는 BDD를 중요시 한다.
![xp](_assets/xp.png)

## 2. XP 의 특징
* 5 ~ 10명 정도로 구성된 프로그래머가 고객과 함께 한 장소에서 일함
* 개발은 점진적으로 이루어지고(With TDD), 각 단계마다 기능을 추가해 나간다.
* 요구사항 정의시 ***사용자***가 새로운 기능을 ***스토리형태(User Story)***로 구체적으로 작성함
* 프로그래머는 엄격한 코딩 규칙을 준수하고 짝(Pair)을 이루어 개발 및 테스트를 함
* 요구사항, 아키텍쳐, 디자인은 ***언제든지*** 변경될 수 있다.

!> Iterator 와 Sprint, Cycle은 비슷한 의미이다.

## 3. 5가지 핵심가치
* 의사소통(Communication) : 팀 개발에 가장 중요한 요소.
  * 개발 과정의 문제는 이미 해결책을 알고 있는 경우가 많음
  * 상호간 의사소통을 통해 문제 해결
  * One site customer, user stories, pair programming, collective ownership, daily standup meetings 등
* 단순성 (Simplicity)
* 피드백 (FeedBack) : 빠르게 그리고 직설적으로
  * 고객에게 되도록 빨리 SW 를 보여주고 필요한 변경은 피드백 받는 것이 최선이다. 그렇기에 온사이트에 Client가 있는게 중요하게 여겨진다.
  * 고객 => 개발자: 관심을 끄는 혹은 쓸만한 기능들
  * 단위 테스트 => 개발자: 시스템의 현재 상태
  * 시스템과 코드 => 관리자들: 개발의 진척 상태
* 용기 (Courage): 무언가 두려운 것에 직면했을 때 필요한 것.
  * 필요한 것에만 집중
  * 피드백에 대해 의사소통 하고 받아들이는 것
  * 진척과 추정에 대해서 진실을 말하는 것
  * 작성된 코드를 리팩토링 하는 것
  * 변화를 두려워하지 않는 것
  * 프로토타입의 코드를 버리는 것
* 존중 (Respect)

## 4. 실천방법
1) Planning Game
* planning poker
*  반복을 위한 계획
*  사용자 스토리는 고객(Client)가 제공함
*  비즈니스 / 개발의 책임 분리
   *  비즈니스: 범위, 우선순위, 릴리스의 구성, 릴리스 일자
   *  기술: 구현에 대한 추정, 구현의 결과, 일의 절차, 상세한 일정에 대한 결정 등을 필요로 함.
* 개발자들이 일수를 지정하고, 이에 대해 개발자들간 일정이 왜 이렇게 걸릴지 이야기 하면서 일정을 조정한다.

1) Small Release
* 작은 기능들로 릴리스 하며 자주 배포해야 한다.

3) Metaphor
* 이미 익숙한 사물에 비유
* 메타포는 전체 시스템을 하나로 묶는 큰 그림이며
* 개별적인 모듈의 위치와 형태를 명백하게 만드는 시스템의 버전이다.
* 애자일은 운전이다.

4) Simple Design
* XP팀은 프로젝트를 가능한 단순하게 설계해야 하며
* 팀의 첫번째 행동은 '가능한 가장 단순한 방식으로 동작하는 스토리의 첫 묶음을 얻어내는 것
  * 어떻게든 동작하는 가장 단순한 것을생각
  * 필요하지 않을 것이라는 가정에서 시작
  * 코드를 중복해서 쓰지 않음

5) Testing
* 단위 테스팅
* 테스트를 우선으로 하는 설계
* 모두 자동화

6) Refactoring
* 외부에 영향을 주지 않고 시스템 구조를 개선시키는 일련의 작은 변환

7) Pair Programming
* 하나의 컴퓨터 하나의 키보드, 두명의 개발자
* 한 명은 코드 작성, 다른 개발자는 에러와 개선점을 찾음
* 전문성이 요구되는 작업은 전문가에게 맡기기도 함
* 효율성이 떨어질 것 같으나 연구 결과로는 오히려 결함을 줄여준다고 함
* 팀원간 갈등이 많이 생기기도 함

8) Collective Ownership
9) Continuous Integration
* 작성된 코드를 하루에도 몇 번씩 통합하고 빌드하여 테스트 해야한다

10) On-Site Customer
* 개발팀이 궁금해하는 것, 분쟁이 생긴 요구사항, 우선순위 선정 등에 답을 줄 수 있는 고객이 팀에 존재하면 좋다
* 고객은 1주일에 40시간을 이 역할에 할애하지 않아도 된다.
* 개발팀에 빠르고 지속적인 피드백을 제공하면 된다.

11) 사용자 스토리
```
As a <role>
I want <goal>
so that <benefit>
Acceptance criteria:
```
* 시스템 사용자 혹은 구매자에게 가치가 될 만한 기능을 기술한 것
* 상세 내용을 구체화하는데 도움이 되는 이야기
* 사용자 스토리는 고객 요구사항을 문서화시키는것이 아닌 요구사항(User back log) 자체를 나타냄
* 사용자 스토리는 요구사항에 대한 텍스트를 포함하고 있지만 세부 사항은 결국 의사소통을 통해 파악하고 확인하는 과정이 필요함
* 스토리 간에 의존성을 회피해야 하며(독립적) => release plan을 통해 매 스프린트마다 잘 고려를 하여 넣어야 한다.
* 스토리 카드는 기능에 대한 짧은 기술을 표현하고 고객과 개발자 간에 대화를 통해 세부 사항을 협상해야 한다
* 각각의 스토리는 고객이나 사용자에게 가치가 있어야 하며
* 스토리가 동작하는 코드로 바뀌는데 거릴는 시간을 예측할 수 있어야 함(Biz to Source code)
* 스토리에 대한 구현이 완료되었다는 기준을 결정하기 위한 테스트 방법이 있어야 함

12) Persona
* 어떤 제품 혹은 서비스를 사용할 만한 목표 인구 집단 안에 있는 다양한 사용자 유형들을 대표하는 가상의 인물
* 페르소나는 브레인스토밍이나 유스케이스 분석 혹은 기능 정의 등의 개발 과정에 다양하게 쓰일 수 있음
* 개발팀 내부에서 대상 사용자 그룹들을 특정함으로써 다양한 소비자들의 니즈를 이해하는데 사용된다.

## 5. 역할
|  <center>개발자</center> |  <center>고객</center> |  <center>관리자</center> |<center>코치</center> |
|:--------|:--------:|--------:|--------:|
|**스토리 추정** | <center>사용자 스토리 작성 </center> |*Planning game의 규칙을 정함* |*XP 이해, 전파* |
|**작업 추정** | <center>사용자 스토리 설명</center> |*Release planning과  Iteration Planning회의 진행* |*상황에 맞는 실천방법을 팀에 알려줌* |
|**코드작업** | <center>기능 테스트 진행</center> |*기능 테스트의 오류 추적* |*팀이 문제를 해결하도록 지원* |
|**단위 테스트** | <center>우선순위 설정</center> |*정보를 수집하고 배포* ||


![xp](_assets/xp.png)