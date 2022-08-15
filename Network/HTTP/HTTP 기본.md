> 이 글은 김영한님의 [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard) 강의의 내용을 바탕으로 정리했습니다.


# HTTP 기본
- [HTTP특징](#http-특징)
- [클라이언트 서버 구조](#클라이언트-서버-구조)
- [무상태 프로토콜(Stateless)](#무상태-프로토콜stateless)
- [비 연결성(Connectionless)](#비-연결성connectionless)
- [HTTP 메시지](#http-메시지)
</br></br>

## HTTP 특징
HTTP는 **H**yper **T**ext **T**ransfer **P**rotocol의 약자로써,</br>
**인터넷에서 데이터를 주고받을 수 있는 프로토콜(통신규약)** 이라는 뜻을 가지며</br>

다음과 같은 특징을 가지고 있다.
- HTML, TEXT, 이미지, 영상, 파일 등 거의 모든 형태의 데이터를 전송할 수 있다.
- 서버간 데이터를 주고 받을 때에도 대부분 HTTP를 사용
- TCP는 `HTTP/1.1`, `HTTP/2`</br>
UDP는 `HTTP/3`을 기반 프로토콜로 사용</br>

- 클라이언트 서버 구조로 동작
- 무상태 프로토콜을 지향, 비연결성
- HTTP 메시지를 통해서 통신
- 단순하고 확장이 가능

</br></br></br>

## 클라이언트 서버 구조
- 클라이언트는 UI와 같은 기능, 서버는 비지니스 로직, 데이터과 같은 기능만 담당하게 분리한다.
- 클라이언트와 서버가 서로 독립적으로 진화가 가능하며, 변경사항이나 문제가 생겼을 시 해결하기 편리하다.

<img src = "https://user-images.githubusercontent.com/84119178/183246002-6115c899-7a5d-4336-a823-5147a7f21e11.png" width = "600" height = "146">

1. 클라이언트가 서버에 요청을 보내고 응답을 대기한다.
2. 서버가 요청에 대한 결과를 만들어서 응답한다.

</br></br></br>

## 무상태 프로토콜(Stateless)
서버가 클라이언트의 상태를 보존하지 않는다.</br>
장점 : 서버 확장성이 높음(스케일 아웃)</br>
단점 : 클라이언트가 추가 데이터를 전송</br>
</br></br>

### Stateful, Stateless 차이

<img src = "https://user-images.githubusercontent.com/84119178/183833030-bc3bbd2a-66f7-40f0-86eb-27790d9feb3d.png" width = "500">

**Stateful(상태 유지)** : 중간에 다른 서버로 바뀌면 안된다.</br>
(중간에 다른 서버로 바뀔 때 상태 정보를 다른 서버에 미리 알려야 한다.)</br>

</br></br>

<img src = "https://user-images.githubusercontent.com/84119178/183834283-68054806-4a83-4afb-ab93-4e16b5cd6a97.png" width = "500">

**Stateless(무상태)** : 중간에 다른 서버로 바뀌어도 된다.
- 갑자기 클라이언트 요청이 증가해도 서버를 대거 투입 가능.
- 무상태는 응답 서버를 쉽게 바꿀 수 있다. -> 무한한 서버 증설 가능</br>

- **한계**
    - 모든 것을 무상태로 설계할 수 있는 것도 있고, 없는 경우도 있다.
    - 상태유지 -> ex) 로그인
    - 무상태 -> ex) 로그인이 필요없는 단순한 서비스 소개 화면
    - 일반적으로 브라우저 쿠키와 서버 세션 등을 사용해서 상태 유지
    - 무상태는 데이터를 너무 많이 보냄
    - 상태 유지는 최소한으로 사용하는 것이 좋다.

</br></br></br>

## 비 연결성(connectionless)
<img src = "https://user-images.githubusercontent.com/84119178/183885241-558f506e-6346-411b-bbbf-d6bfb17c68a8.png" width = "500" height = "288"><img src = "https://user-images.githubusercontent.com/84119178/183885408-7868ece9-c18e-4ff4-8e7b-a897bc3aba59.png" width = "500">
</br></br>

<img src = "https://user-images.githubusercontent.com/84119178/183885836-77de0178-8534-4870-b897-a4906776a310.png" width = "500" height = "287.3"><img src = "https://user-images.githubusercontent.com/84119178/183886661-e2686191-d60a-42d9-b947-058964e828d0.png" width = "500">

### **특징**
- HTTP는 기본적으로 연결을 유지하지 않는다.
- 보통 초 단위 이하의 빠른 속도로 응답한다.
- 서버 자원을 매우 효율적으로 사용할 수 있다.
- 수천개의 요청이 들어와도 동시에 처리하는 요청은 수십개 이하로 매우 작다.

### **단점**
- TCP/IP 연결을 매번 새로 맺어야 한다 -> 3 way handshake 시간 추가
- 웹 브라우저로 사이트를 요청하면 HTML뿐만 아니라 JS,CSS, 추가 IMG등등 수 많은 자원이 함께 다운로드
- 지금은 HTTP 지속연결로 문제 해결

    <img src = "https://user-images.githubusercontent.com/84119178/183888912-c6eb16d3-5bec-4255-b555-30d79afad35c.png" width = "450">
    </br>
    <img src = "https://user-images.githubusercontent.com/84119178/183889434-dcd21e3a-02f4-4d83-9e3a-e64c557307e8.png" width = "500">

</br></br></br>

## HTTP 메시지
HTTP 메시지에 거의 모든 형태의 데이터를 전송할 수 있다.</br>

서버간에 데이터를 주고 받을 떄도 대부분 HTTP를 사용한다.</br>

### HTTP 메시지 구조
<img src = "https://user-images.githubusercontent.com/84119178/183892132-5ee7c810-4701-4cf7-adea-c57ede1d7191.png" width = "800">

</br></br>

## 시작라인(start - line)

### 요청 메시지
<img src = "https://user-images.githubusercontent.com/84119178/184471613-51cbe00b-0215-4344-b4eb-96861b669640.png" width = "300">

1. HTTP 메서드(중요)</br>
**GET** `/search?q=hello&hl=ko HTTP/1.1` </br>
    - 종류 : GET, POST, PUT, DELETE 등등
    - 서버가 수행해야 할 동작을 지정한다.
        - GET : 리소스 조회
        - POST : 요청 내역 처리</br>

2. 요청 대상</br>
`GET` **/search?q=hello&hl=ko HTTP/1.1** </br>
    - absolute-path[?query] - (절대경로[?쿼리])의 형태로 이루어 진다.
    - 절대경로 : "/"로 시작하는 경로
    - 다른 유형의 경로지정 방법도 있다.

3. HTTP 버전</br>
`GET /search?q=hello&hl=ko` **HTTP/1.1** </br>
</br></br>

### 응답메시지
<img src = "https://user-images.githubusercontent.com/84119178/184471742-ec2f5296-70b3-4cd0-9a28-674ac94d325a.png" width = "300">

1. HTTP 버전</br>
**HTTP/1.1** `200 OK`

2. HTTP 상태 코드</br>
`HTTP/1.1` **200** `OK`</br>
요청 성공, 실패를 나타냄
    - 200 : 성공
    - 400 : 클라이언트 요청 오류
    - 500 : 서버 내부 오류

3. 이유 문구</br>
`HTTP/1.1 200` **OK**</br>
사람이 이해할 수 있는 짧은 상태 코드 설명 글
</br></br>

#### **HTTP 헤더**
<img src = "https://user-images.githubusercontent.com/84119178/184471801-624146cc-7682-48b8-a7ef-ea97bf836adb.png" width = "600"> 

**field-name ":" OWS field-value OWS** </br>
(OWS : 띄어쓰기 허용)구조로 이루어지고 field-name은 대소문자 구분X
- HTTP 전송에 필요한 모든 부가정보가 들어가 있다.</br>
(메시지 바디를 제외하고 필요한 모든 메타데이터 정보가 들어가 있다.)
- 표준 헤더가 너무 많다.
- 필요시 임의의 헤더를 추가할 수 있다.
</br></br>

#### **HTTP 메시지 바디**
<img src = "https://user-images.githubusercontent.com/84119178/184472167-2100923b-7177-4405-8f9c-23a78388e2cb.png" width = "350">

- 실제로 전송할 데이터들
- HTML, 이미지, 영상 등등 모든 데이터 전송 가능


