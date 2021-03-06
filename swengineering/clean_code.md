###### 2019.06.19

## 1. 클린 코드란? 
코드를 작성하는 의도와 목적이 명확하며 다른 사람이 쉽게 읽을 수 있어야 한다.
`단순` `읽기 쉬움` `주의 깊음`

?> <b>가독성의 기본 정리(The Fundermental Theorem of Readability)</b>  
코드는 다른 사람이 그것을 이해하는데 들이는 시간을 최소화하는 방식으로 작성되어야 한다.

## 2. 왜 클린 코드가 필요할까?

 개발시간을 단축하기 위해

`코드를 읽는 시간 : 코드를 짜는 시간 = 10 : 1`

새 코드를 짜면서 끊임없이 기존 코드를 읽기 때문에 기존 코드를 쉽게 읽을 수 있어야 짜는것도 쉬워진다.

## 3. 클린코드를 위한 큐칙

### 1) 의미 있는 네이밍

* 변수, 클래스, 메서드에 의도가 분명한 이름을 사용해야 한다.  
* 잘못된 정보를 전달할 수 있는 이름은 사요아지 않는다.
  * 범용적으로 사용되는 단어를 다른 의미로 사용하지 않는다.
  * 가독성이 떨어지는 문자를 사용하지 않는다.

### 2) 명확하고 간결하게 주석달기  

주석은 코드를 읽는 사람이 코드를 작성한 사람만큼 코드를 잘 이해할 수 있도록 돕는다.  
그래서 함수, 메서드, 객체 등의 중요한 세부 사항만을 주석으로 남기는 것이 좋다.  
또는 이에 대한 간략한 설명을 기입한다. 

### 3) 보기좋게 배치하고 꾸미기

* 보기 좋은 코드가 읽기도 좋기 때문에 규칙적인 들여쓰기와 줄바꿈으로 가독성을 향상시켜야 한다.
* 메서드를 이용하여 불규칙한 중복 코드를 제공한다.
* 가독성에 향상이 된다면, 열을 맞추는 것도 좋다.
* 코드를 그룹과 계층 구조 방식으로 조작하면 가독성이 향상된다.
  * 클래스 전체를 하나의 그룹으로 묶는 것 보다 여러 개의 그룹으로 나누는 것이 읽기 좋다.
* 코드를 문단으로 나눠서 작성한다.

### 4) 읽기 쉽게 흐름 제어 만들기

* 왼쪽에 변수를, 오른쪽에 상수를 두고 비교하라
* 부정이 아닌 긍정을 다뤄라
* 조건문을 사용할때 매우 간단한 경우에만 삼항 연산자를 사용하라.
* do while 루프 피하기
* 중첩을 최소화하기
* 설명변수와 요약 변수를 이용하여 커다란 표현을 작게하라 => 너무 요약된 변수는 클린코드에 적합하지 않을 수 있다.

### 5) 착한 함수 만들기

* 함수는 가급적 작게, 한번에 하나의 작업만 수행하도록 작성되어야 한다.

## 4. 레거시 코드를 다루기 위한 프랙티스

누가 작성해서 사용하고 있는 코드인데, 퀄리티가 떨어질 경우 어떻게 고쳐야 하는가?

### 1) 코드 리뷰  

* 코드를 작성하기 위한 규칙(코딩 컨벤션 - 표준 코딩 정의서)을 준수하고 주석등에 신경을 씀으로써 `가독성`이 향상 된다.  
* 코드 인스펙션: 작성한 개발소스 코드를 분석하여 개발 표준에 위배되었거나 잘못 작성된 부분을 수정하는 작업  
* 지속적인 코드 인스펙션을 위한 솔루션
  1. 개발자는 자신이 개발한 소스 코드를 개발 브랜치로 푸시한다.
  2. CI 서버는 형상관리서버 Git 저장소를 트리거 하여 빌드를 수행한다.
  3. CI 서버는 빌드검증 결과와 단위테스트 결과를 형상 관리 서버에게 알려준다.
  4. 코드 감사 결과를 인스펙션 서버에 업데이트 한다.
  5. 개발자는 개발 브랜치에 개발한 코드를 마스터 브랜치로 머지하기 위해 풀 리퀘스트를 수행한다.
  6. 인스펙션서버는 코드 감사결과를 풀리퀘스트에 업데이트 한다.
  7. 리뷰어는 코드 감사 결과를 확인하고 소스코드를 머지한다.

코드리뷰 체크리스트
```
* 코딩 스타일가이드를 준수하고 있는가?
* 하나의 함수가 하나의 기능만을 수행하고 있는가?
* 각 함수의 정보는 코드를 설명하기에 충분한가?
* 주석은 적절하게 기술 되어 있는가?
* 코드는 잘 구조화되어 있는가? (가독성과 기능적 측면)
* 헤더, 함수 정보를 도구로 추출해서 자동으로 문서화할 수 있는 구조인가?
* 함수와 변수의 이름은 일관되고 상세하게 기술되어 있는가?
* 숫자를 직접 서술하지 않고 상수 변수를 사용하는가?
* 주석처리 된 코드를 삭제 하였는가?

* 리소스에 lock 이 있다면 unlock 은 이루어져있는가?
* 스택변수는 반환하고 있는가?
* 입력 파라미터의 유효범위는 체크하고 있는가?
* 오류코드를 반환하는 함수보다 예외를 적극적으로 활용하고 있는가
```

### 2) 리팩토링  

프로그램 결과에는 변화를 주지않고 내부구조를 변경하는 작업.

**메서드 정리** ,
**객체간 기능 이동**, 
**데이터 구성** ,
**조건문 단순화**,
**메서드 호출 단순화**, 
**클래스 및 메서드 일반화**