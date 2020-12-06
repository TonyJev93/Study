# TDD

## TDD 란?
- Test Driven Development
- test-then-code approach
- 매우 짧은 개발 사이클을 반복하는 소프트웨어 개발 프로세스 중 하나
 

## 특징

- 소프트웨어를 더 자주 빠르게 만듦.
- 테스트를 먼저 작성하게 하므로서 구현을 위해 어떤것들이 필요하고 어떻게 사용 될지 설계에 대해 고려 하도록 만듦.
- Agile 방법론에 적합함. (test-then-code & short development cycles or sprints 의 시너지 효과)

## 개발 방식

- 함수 작성 전에 테스트 케이스를 먼저 이용
- 테스트 실패 시 -> 무엇이 변경되었는지 인지 -> Refactoring 수행 (즉, 함수 내 알림 없이도 결함 발견 가능) 
- Red / Green / Blue

### 장점

-  **Deveolop highly usable software**
    - 구현보다는 사용에 초점을 둔다.
    - 테스트 접근을 통한 개발자로 하여금 사용자가 직면할 사용성에 대한 문제를 더 고려하도록 만든다.
- **Reduce bugs**
    - 테스트 자동화를 통해 실수가 존재하는 부분 발견하고 집중할 수 있다. 
       -> 사용자가 코드 변경으로 발생하는 버그와 다른 문제들을 감지 할 수 있도록 함.
    - 소프트웨어를 좀 더 solid 하고 의도한데로 개발 할 수 있다.
- **Identify functionality issues**
    - 개발 도중에도 기능성 문제를 직시할 수 있다.
    - 완벽한 출시가 필요한 경우 TDD를 사용할 수 있다.
- **Avoid code duplication**
    - 지속적인 Refactoring을 통해 코드를 Clean UP 할 수 있다
    - 사용 될 코드를 미리 배치함에 따라 코드 중복을 피함.
- **Simplify code**
    - 테스트 요구사항에 맞도록 개발자가 코딩하도록 하여 더욱 심플한 코드를 짜도록 장려한다.
    - 과도한 코드를 방지하고 코드베이스를 능률적이고 사용하기 쉽게 유지하는데 도움이 됨.
    - 불필요한 코드가 침투하는 것을 막는다.
- **Check the quality of the code**
    - 코드의 품질을 판단하는데 사용.
    - 일관된 코딩과 발견된 결함을 수정할 수 있음.
- **Create extensible software**
    - TDD는 작은 단위의 테스트 가능한 코드를 통해 소프트웨어를 빌드하도록 장려된다.
    - 이러한 모듈화 접근은 소프트웨어를 좀 더 유연하고 확장성 있도록 만든다.
- **Provide quick feedback**
    - 빠른 반복 개발을 통해 클라이언트로부터 빠른 피드백을 받을 수 있다.
    - 이를 이용하여 좀 더 새련된 소프트웨어 제작이 가능하다.
    - 클라이언트에게도 이득임
- **Create up-to-date code documentation**
    - 완료된 테스트 코드는 Use Case로 사용되어 소프트웨어에 더 빨리 친숙하도록 만든다.
    - 많은 사람의 손을 거쳐가는 장기간의 프로젝트에서는 테스트 케이스가 가치있는 문서가 될것이다.
- **Shorten the development time to market**
    - 개발 팀으로부터 테스트 통과에 집중하도록 하여 생산성을 높힌다.
    - 다른 방식에 비해 TDD는 end-user를 위한 기능을 개발할 수 있도록 만든다.
    - 사용성이 중시되는 요즘 시장에 적합한 이점이다.

### 언제 사용 할까?

- New Software & Legacy Software 둘 다 사용 가능
- Legacy에 적용 시, TDD는 테스트를 사용하여 버그를 하나씩 해결할 수있는 방식으로 버그를 개별적으로 해결한다.
- 적합한 사용의 예시
    - Greenfield Software project : 제약사항이 없이 새로운 프로젝트를 개발하는 경우
    - 형식적인 것보다 기능이 우선되는 프로젝트
    - 짧은 구현 기간을 요구하는 프로젝트
    - 확장성있고 지속적 개발 가능한 모듈을 요구하는 장기 프로젝트
    - 비용과 기능 측면에서 최적화 해야하는 프로젝트(사용 가능한 예산에서 가능한 최상의 기능을 얻는 것이 목표)


### 한계점

- TDD는 (database-reliant Software가 요하는) Full functional test를 뜻하지 않음.
- 통합 테스트와 구분해야 한다.
- TDD는 명확한 요구사항을 필요로하며, 개발자가 이를 이해해야 한다. (그렇지 않으면 단위 테스트의 효과가 없다.)
- 많은 수의 테스트 수행은 시간과 비용을 발생시키며 이로인한 매우 복잡한 앱이 될 수 있다. 
  


### 고찰 (참고 글에 작성된 고찰임)

- TDD는 신뢰할 수 있고 가용적이며, 새로운 기능으로의 확장을 용이하게 해준다.
- TDD는 짧은 피드백 loop를 통해 '무엇이 중요한가'에 대해 포커스를 맞추게 하여 최적화된 비용이 들도록 돕는다.
- 신규 프로젝트를 시작하던 기존 시스템을 현대적인 방식으로 바꾸던 TDD는 적합하다.
- 결론, TDD는 더 좋은 app개발을 제안할 뿐 마법적으로 모든 소프트웨어의 문제를 해결해주지는 않는다.
- 명확한 요구사항을 마련하고 명확하게 전달하는 것이 중요한 숙제이다.

## 참고
- TDD 설명 : https://marsner.com/blog/why-test-driven-development-tdd/