* 온프레미스

온프레미스(On-premise)란 소프트웨어 등 솔루션을 클라우드 같이 원격 환경이 아닌 자체적으로 보유한 전산실 서버에 직접 설치해 운영하는 방식을 말합니다. 온프레미스는 클라우드 컴퓨팅 기술이 나오기 전까지 기업 인프라 구축의 일반적인 방식이었기 때문에 이전 또는 전통적인 이라는 단어와 함께 사용됩니다.

클라우드 사용 이후, 온 프레미스 환경에서 동작하던 작업방식들이 바뀌게 되는데,  실행 환경 구축 범위가 간소화되어 짧은 사이클로 릴리스를 반복하는 형태 => 애자일 개발 방법론이 선호되게 됨

클라우드를 구성하는 대부분의 기술은 한 대의 물리 호스트 상에서 움직이는 시스템과는 달리 분산 환경에서 가동시키는 것이 기본.

그렇기에 수동 운영(Operation) 을하지 않고 자동화 된 툴을 사용하여 오케스트레이션(orchestration) 을 진행함.



* 시스템 기반

  애플리케이션을 가동시키기 위해 필요한 하드웨어, OS, Middle ware와 같은 인프라. 

  * 기능 요구 사항 : 시스템의 기능으로서 요구 되는 사항들. 시스템이나 소프트웨어에서 무엇을 할 수 있는지 모아놓은 것
  * 비기능 요구 사항 : 시스템의 성능, 신뢰성, 확장성, 운용성, 보안 등.  기능 요구 사항 외의 것들
  * 미들웨어: 서버 OS 상 서버가 특정 역할들 다하기 위한 기능을 갖고 있는 소프트웨어. 

* 시스템의 이용 형태

  * 온 프레미스(on-premises): 자사에서 데이터센터를 보유하고 시스템 구축부터 운용까지 모두 수행하는 형태. 초기 비용이 크며, 시스템 가동 후 운용에 드는 비용도 어느 정도 있음
  * 퍼블릭 클라우드: 인터넷 경유, 불특정 다수에게 제공 되는 클라우드 서비스. AWS, AZURE, FIREBASE 등
  * 프라이빗 클라우드: 특정 기업 그룹에게만 제공되는 클라우드 서비스. 기업 내 데이터센터를 공동으로 보유하는 듯한 이미지

정확한 트래픽을 예측하기 어려울 때 Public Cloud를 사용하는게 제법 괜찮은 선택임.

**사이징(Sizing) **: 트래픽 양에 따라 시스템 기반의 서버 사양이나 네트워크 대역을 가늠하는 설계. 인프라 설계에서 난이도 높은 기술 중 하나가 사이징. 그렇기에 클라우드 서비스를 활용한 **오토스케일링** 은 정말 강력한 기능임.

그러나 때로는 온프레미스가 적합한 케이스가 있는데,

* 높은 가용성이 요구되는 시스템 : 미션 크리티컬 시스템의 경우, 서버가 끊어지면 안되는 서비스 이기에 클라우드 업체가 보장하는 것 이상 가용성이 필요한 시스템의 경우 온 프레미스로 구성하는게 더 낫다.
* 기밀성이 높은 데이터를 다루는 시스템: 물리적인 저장장소를 명확히 할 필요가 있는 경우
* 특수한 요구사항이 있는 시스템 : 범용적이지 않은 디바이스 또는 특수한 플랫폼에서만 움직이는 시스템 구축, 이전 할 경우.