# OAuth

## OAuth 란?

- OpenID Authentication
- OAuth 사용 전 인증 표준이 없어 ID/PW 인증방식 사용 -> 보안상 취약점이 많음
- ID/PW 인증방식이 아닌 각 애플리케이션들이 각자의 개발한 회사의 방법대로 사용자를 확인함<br>
  ex) 구글 : AuthSub, AOL : OpenAuth, 야후 : BBAuth, 아마존 : 웹서비스 API 등
- OAuth는 이렇게 제각각인 인증방식을 표준화한 인증방식
- OAuth 이용하면 이 인증을 공유하는 애플리케이션끼리는 별도의 인증이 필요없이 여러 애플리케이션을 통합하여 사용 가능


## 생활코딩

### 참가자

1. 우리 서비스
2. 사용자
3. 외부 서비스 (Google, Facebook, Twiiter, ...)

- OAuth 전
    - 사용자 -> 우리 서비스 : ID / Password 전달 -> 외부 서비스 접속
        - 단점 : 우리 서비스가 사용자의 계정정보를 이용하여 외부 서비스를 호출한다는 점 (제 3자가 사용자의 계정정보 소유 -> 신뢰 X)
- OAuth 후
    - 외부 서비스 -> 우리 서비스 : accessToken 전달(ID/PW가 아니고, 우리 서비스가 필요로 하는 기능만 제공)

### 용어

- mine(우리가 만든 서비스) = Client
- User(사용자) = Resource Owner
- Their(외부 서비스) = Resource Server_데이터를 가지고 있는 서버 (Authorization Server_인증서버)

### 절차

1. 등록
    - Client -> Resource Server
        - Client ID : 애플리케이션을 식별하는 ID (외부노출 O)
        - Client Secret : 애플리케이션 비밀번호 (외부노출 X)
        - Authorized redirect URIs : Authorized Code를 전달 받을 주소
2. 인증
    -  Resource Owner -> Client : 로그인 요청
        - Client -> Resuorce Owner : 로그인 화면 전달
            - Resource Owner -> Client : client_id & scope & redirect_uri
            - Resource Owner -> Resource Server : 로그인 접속화면으로 이동 -> 로그인 완료 -> 승인할 내용 출력 : 승인한 scope 정보가 Resource Server DB에 저장 됨.
                - Resource Server -> Resource Owner : scope 사용을 허용하기 위한 토큰 발행 전 authorization code 전달(헤더에 Location(리다이렉팅)을 통해 Client에 전달)
                - Client -> Resource Server : grant_type & authorization_code & redirect_uri & client_id & client_secret
                - Access Token 발급 : Authorization Code 삭제 후 Access Token을 Client한테 전송 ( Cliet는 해당 토큰 저장 )
                    - Resource Server : AccessToken에 맵핑된 사용자 id와 scope를 통해 Resource Owner의 권한 체크                    
3. API 사용
    - Resource Server가 Client에게 API 제공
        - Query parameter or Authorization: Bearer 이용하여 AccessToken 담아서 서비스 요청
4. Refresh 토큰
    - AccessToken = 수명이 존재, 수명이 만료되면 Refresh 토큰을 통해 Access Token을 재발급
    

### Federated Identity : 연합 식별

- login with facebook / twitter / google

### 궁극 목적
- Restful API 사용

## 참고

- 개요 : https://velog.io/@namezin/OAuth
- Google API OAuth2.0 연동 : https://developers.google.com/identity/protocols/oauth2
- 생활코딩 : https://opentutorials.org/course/3405
- 스프링 시큐리티 + OAuth2.0 연동 : [velog.io](https://velog.io/@swchoi0329/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%8B%9C%ED%81%90%EB%A6%AC%ED%8B%B0%EC%99%80-OAuth-2.0%EC%9C%BC%EB%A1%9C-%EB%A1%9C%EA%B7%B8%EC%9D%B8-%EA%B8%B0%EB%8A%A5-%EA%B5%AC%ED%98%84)
