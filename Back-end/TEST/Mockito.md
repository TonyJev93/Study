# Mockito

## 소개
- Mock : 진짜 객체와 비슷하게 동작하지만 프로그래머가 직접 객체의 행동을 관리하는 객체
- Mockito : Mock 객체를 쉽게 만들고 관리하고 검증할 수 있는 방법 제공
- 테스트 작성 개발자 50% 이상이 사용하는 Mock 프레임워크
- 대체제 : EasyMock, JMock
- 활용 : 외부 API와 연동 테스트(가상)


---
## Mock 객체 Stubbing
- **Mock 객체의 행동**
    - Null 리턴(Optional 타입은 Optional.empty)
    - Primitive 타입은 기본 Primitive 값
    - 콜렉션은 비어있는 콜렉션
    - Void 메소드는 예외를 던지지 않고 아무런 일도 발생하지 않는다.
- **Argument Matcher**
    - 참고 url : https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html#3
- **when()** : return 타입이 있을 때 활용
- **doThrow()** : return이 void 인 경우 예외처리 


---
## 객체 확인

- Method 수행여부 체크
    - verify(xxxService, times(n)).methodName(xxx) : method가 n번 수행 되었는지 확인
        - times -> never : 한번도 수행되지 않았는지 확인
- 수행 순서 체크
    - InOrder inOrder = inOrder(xxxService);
    - inOrder.verify(xxxService).methodName(xxx); // 작성한 순서에 맞게 수행됬는지 확인 
    - inOrder.verify(xxxService).methodName(yyy); 
- 수행 중단여부 체크
    - verifyNoMoreInteractions(xxxService); // 해당 함수가 선언된 이후 xxxService의 서비스가 호출되면 오류 발생

---
## Mocikto BDD API
- BDD : 애플리케이션이 어떻게 '행동'해야 하는지에 대한 공통된 이해를 구성하는 방법
- TDD에서 창안
- 테스트케이스로 Spec 정의
- BDD API 사용(Given -> When -> Then)
    - when() -> given() : BDD API 
    - verify -> then().should(times)
    - verifyNoMoreInteractions() -> then().shouldHaveNoMoreInteractions()


--- 

### 예제
-  https://github.com/keesun/inflearn-the-java-test (출처 : 백기선 git)
## 출처
- https://docs.google.com/document/d/1j6mU7Q5gng1mAJZUKUVya4Rs0Jvn5wn_bCUp3rq41nQ/edit#
