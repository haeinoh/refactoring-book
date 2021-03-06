# CH02 리팩터링의 원칙

### 2.1 리팩터링 정의

- 리팩터링: [명사] 소프트웨어의 겉보기 동작은 그대로 유지한 채, 코드를 이해하고 수정하기 쉽도록 내부 구조를 변경하는 기법
- 리팩터링(하다): [동사] 소프트웨어의 겉보기 동작은 그대로 유지한 채, 여러가지 리팩터링 기법을 적용해서 소프트웨어를 재구성하다.
  - 재구성(restructing): 코드베이스를 정리하거나 구조를 바꾸는 모든 작업
  - 겉보기동작(observable behavior) : 리팩터링하기 전 후의 코드가 똑같이 동작해야 한다.



### 2.2 두 개의 모자

- 기능 추가 : 기존 코드 건드리지 않고, 새 기능만 추가
- 리팩터링 : 기능 추가 하지 않고, 코드 재구성 (되도록이면 테스트도 만들지 않는다)



### 2.3 리팩터링하는 이유

- 소프트웨어 설계가 좋아진다
  - 설계가 나쁨 -> 코드가 길어짐 -> 중복 코드 발생 확률 높고, 수정하기 힘듦
- 소프트웨어 이해가 쉬워진다
  - 의도 명확하게 전달 가능
- 버그를 쉽게 찾을 수 있다
  - 코드 이해하기 쉬움 -> 버그 찾기 쉬움
- 프로그래밍 속도를 높일 수 있다
  - 내부 설계가 잘 된 소프트웨어는 기능 수정 하기 쉬움
  - 모듈화가 잘 되어 있음 -> 이해 쉬움
  - 코드가 명확 -> 버그 찾기, 디버깅 쉬움
- 위의 효과들을 *"지구력 가설"*이라고 부른다 
  - 소프트웨어의 지구력이 높아서 빠르게 개발할 수 있는 상태를 유지



### 2.4 언제 리팩터링해야 할까?

- 준비를 위한 리팩터링 : 기능을 쉽게 추가하게 만들기

  - 가장 좋은 시점 - 코드베이스에 기능 새로 추가하기 직전
  - 함수 매개변수화하기

- 이해를 위한 리팩터링 : 코드를 이해하기 쉽게 만들기

  - 코드 의도 파악
  - 세부 코드에 이해를 위한 리팩터링 -> 변수 이름, 긴 함수 자르기

- 쓰레기 줍기 리팩터링

  - 비효율적 코드 (로직, 중복 함수) -> 즉시 고치거나, 하던 일 마치고 고치기

- 계획된 리팩터링과 수시로 하는 리팩터링

  - 보기 싫은 코드를 발견하면 리퍽터링 하기 

- 오래 걸리는 리팩터링

  - 오래 걸리면 관련된 코드를 작업 중 조금씩 개선

- 코드 리뷰에 리팩터링 활용하기

  - 코드 리뷰의 결과를 구체적으로 도출하는데 도움

- 관리자에게는 뭐라고 말해야할까?

  - 개발자 -> 효과적인 소프트웨어를 빠르게 만드는 역할
  - 리팩터링 -> 소프트웨어 빠르게 만드는데 효과적
  - 개발자 -> 리팩터링 -> 효과적으로 빠르게 새로운 기능을 빠르게 구현 가능  

- 리팩터링 하지 말아야 할 때 

  - 처음부터 새로 작성하는게 쉽다면 리팩터링 하지 않음

  

### 2.5 리팩터링 시 고려할 문제

- 새 기능 개발 속도 저하
  - 리팩터링 궁극적인 목적 : 개발 속도를 높이고, 더 적은 노력으로 더 많은 가치 창출하는 것
  - 리팩터링 본질 :개발 기간 단축 (기능 추가 시간 단축, 버그 수정 시간 단축)
- 코드 소유권
  - 코드 소유권이 나뉘어 있는 경우 : 함수 이름 바꾸기를 적용 -> 기존 함수 유지 -> 함수 본문에서 새로운 함수 호출
- 브랜치
  - 기능 브랜치 방식 단점 : 독립 브랜치 작업 기간 길어질수록 -> 마스터 통합 어려움
  - CI(지속적 통합) 선호 이유 : 머지 복잡도 줄일 수 있고, 리팩터링과 궁합이 좋음
    - 기능별 브랜치는 머지 문제 발생하기 쉬움
    - 컨트 벡이 CI + 리팩터링 -> 익스트림 프로그래밍 (XP)
    - CI 적용 시 기능별 브랜치 사용하는 경우 : 통합 주기 최대한 짧게 하기
- 테스팅
  - 자가 테스트 코드(self-testing code, 스스로 성공/실패 판단하는 테스트) 필요 (안전한 자동 리팩터링 환경이 제공되면 테스트 없어도 됨)
  - 자가 테스트 코드는 통합 과정에서 발생하는 의미 충돌 잡는 메커니즘 활용 가능 -> CI와 연관
  - CI에 통합된 테스트 -> XP 권장사항, CD (지속적 배포)의 핵심
- 레거시 코드
  - 레거시 코드(legacy code) -> 대체로 복잡, 테스트 거의 안 갖춰짐
  - 자주 보는 코드 먼저 개선
- 데이터베이스
  - 프로덕션 환경 -> 여러 단계로 릴리즈 (병렬 수정)

### 2.6 리팩터링, 아키텍처, 애그니(YAGNI)

- 리팩터링 -> 수년 동안 운영된 소프트웨어 아키텍쳐도 변경 가능

- 코딩 전 아키텍쳐 확정 -> 어려움

- 여러 상황 고려하면 유연성 메커니즘 -> 변화 대응 능력 떨어뜨림

- ***간결한 설계***, ***점진적 설계***, ***YAGNI*** : 현재까지 파악한 요구사항만을 해결하는 소프트웨어 구축 -> 진행하면서도 아키텍처 리팩터링

- ***YAGNI***(애그니, You aren't going to need it) : 당장에 필요한 기능만으로 최대한 간결하게 만들어라 (아키텍처 고려하지 말라는 뜻 X)

  - 앞으로 필요할 것 같아서 미리 구현해둔 상당 수의 기능이 결국 쓰이지 않거나, 요구사항을 반영하지 못해서 수정이 어려운 경우가 더 많음

  

### 2.7 리팩터링과 소프트웨어 개발 프로세스

- **익스트림 프로그래밍 (XP)** : 지속적 통합, 자가 테스트 코드, 리팩터링 등 개성 강하면서 상호 의존하는 기법을 묶은 프로세스
- **테스트 주도 개발 (TDD)** : 테스트 코드 + 리팩터링
- **테스트 코드**(프로그래밍 오류 확실히 걸러내는 테스트 자동 수행) + **지속적 통합**(다른 사람의 작업을 방해하지 않으면서 리팩터링, 결과 공유) + **리팩터링** => YAGNI설계 방식 개발 진행



### 2.8 리팩터링과 성능

- 빠르게 소프트웨어 작성 하는 방법

  - 시간 예산 분배 : 컴포넌트마다 자원, 예산 할당
  - 끊임없는 관심
  - 90%의 시간은 낭비 : 의도적으로 최적화에 돌입하기 전까지는 성능에 신경 쓰지 않고 코드를 다루기 쉽게 만드는데 집중 -> 후에 프로파일러로 프로그램 분석하면서 최적화 지점 찾기

  

### 2.9 리팩터링의 유래

- 정확한 유래는 모름
- 스몰토크 개발환경에서 활용 시작



### 2.10 리팩터링 자동화

- 현재 IDE, 에디터, 독립 도구에서 리팩터링 기능을 제공할 정도로 자동 리팩터링 흔해짐
- IDE는 구문 트리를 분석해서 리팩터링 하여 텍스트 에디터보다 훨씬 정교 
















