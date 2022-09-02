> 이 글은 김영한님의 [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard) 강의의 내용을 바탕으로 정리했습니다.

# HTTP 헤더 기본
### 목차
- [헤더 필드의 구조,HTTP BODY](#헤더-필드의-구조host-wwwgooglecombr)
- [표현](#표현)
- [협상](#협상)
- [전송](#전송)
- [일반 정보, 특별한 정보](#일반-정보)
- [쿠키](#쿠키)
---

HTTP 헤더는 다음과 같은 구조를 가지고 있다.

### **헤더 필드의 구조(Host: www.google.com)**</br>
`field-name ":" OWS field-value OWS` (OWS:띄어쓰기 허용)</br>
<img src = "https://user-images.githubusercontent.com/84119178/186613927-c2da2854-f8d0-47bd-84c7-887cb45b276e.png" width = "600">

</br></br>

### **HTTP BODY**
<img src = "https://user-images.githubusercontent.com/84119178/186615214-464617c1-be50-4aad-b913-88b6ab4aaf05.png" height = "200">
- 메시지 본문(Message body)을 통해 표현 데이터 전달
    - 원래는 엔티티(Entity)라고 쓰였지만 최신버전에서는 표현(Representation)이라고 쓰임
- 메시지 본문 = 페이로드(payload) = 실제 데이터 부분
- 표현(표현 헤더 + 표현 데이터) : 요청이나 응답에서 전달할 실제 데이터
</br></br>

<div style="text-align: right"> 

[맨위로](#목차) 
</div>
</br></br>

## 표현
<img src = "https://user-images.githubusercontent.com/84119178/186644703-d5941579-8d3e-418e-b75c-76e8d1350b28.png" width = "600">

1. ### **Content-Type : 표현 데이터의 형식을 설명**
    
    <img src = "https://user-images.githubusercontent.com/84119178/187407823-05041512-68c4-4504-9b16-7ef40c7c26cf.png" width = "300">

- 미디어 타입이나 문자 인코딩 상태를 설명
- ex) text/html; charset=uft-8</br>
        application/json</br>

---

2. ### **Content-Encoding : 표현 데이터 인코딩**

    <img src = "https://user-images.githubusercontent.com/84119178/187408572-3199aff4-1163-4c2d-93d0-6023928df9f2.png" width = "300">

- 표현 데이터를 압축하기 위해 사용된다
- 데이터를 전달하는 곳에서 압축한 뒤, 인코딩 헤더를 추가한다.
- 데이터를 읽는 쪽에서 인코딩 헤더의 정보로 압축 해제

---

3. ### **Content-Language : 표현 데이터의 자연언어**
    
    <img src = "https://user-images.githubusercontent.com/84119178/187408907-fdddbe86-8438-4f75-8fa4-57fc5f83c866.png" width = "300">

- ex) ko, en, en-US.....

---

4. ### **Content-Length : 표현 데이터의 길이**
    - 바이트 단위
    - Transfer-Encoding을 사용할때는 쓰면 안됨!!!
</br>
<div style="text-align: right"> 

[맨위로](#목차) 
</div>
</br></br>

## 협상
클라이언트가 선호하는 표현 요청
- Accept : 클라이언트가 선호하는 미디어 타입 전달
- Accept-Charset : 클라이언트가 선호하는 문자 인코딩
- Accept-Encoding : 클라이언트가 선호하는 압축 인코딩
- Accept-Language : 클라이언트가 선호하는 자연 언어

</br></br>

### **Accept-Language**
<img src = "https://user-images.githubusercontent.com/84119178/187412835-c1506570-93c8-427c-900a-fcf7dfa90683.png" width = "600">

</br></br>

### 협상과 우선순위(Quality Values => q)
- Quality Values(q)값 사용
- `0 <= q <= 1` 클수록 높은 우선순위
- 일반적으로 생략하면 1
- ex)
    - **Accept-Language: `ko-KR, ko;q=0.9, en-US;q=0.8, en;q=0.7`**</br>
        |순위|Accept|Quality Values(q)|
        |:---:|:------:|:---:|
        |1순위|ko-KR|q=1(생략)|
        |2순위|ko|q=0.9|
        |3순위|en-US|q=0.8|
        |4순위|en|q=0.7|

- 구체적인 것이 우선이다.
- ex)
    - **Accept: `text/*, text/plain, text/plain;format=flowed, */*`**</br>
        |순위|Accept|
        |:---:|:------:|
        |1순위|text/plain;format=flowed|
        |2순위|text/plain|
        |3순위|text/*|
        |4순위|\*/\*|
     
- 구체적인 것을 기준으로 미디어 타입을 맞춘다.
- ex)
    - **Accept: `text/*;q=0.3, text/html;q=0.7, text/html;level=1, text/html;level=2;q=0.4, */*;q=0.5`**</br>
        |순위|Accept|Quality Values(q)|
        |:---:|:------:|:---:|
        |1순위|text/html;level=1|q=1|
        |2순위|text/html;level=3|q=0.7|
        |3순위|text/html|q=0.7|
        |4순위|image/jpeg|q=0.5|
        |5순위|text/html;level=2|q=0.4|
        |6순위|text/plain|q=0.3|
</br>
<div style="text-align: right"> 

[맨위로](#목차) 
</div>   
</br></br>

## 전송

### 1. 단순 전송(Content-Length)
<img src = "https://user-images.githubusercontent.com/84119178/187425317-66fbdffd-4b58-4c43-adad-b0dd76c4843f.png" width = "600"></br>
- Content의 길이를 지정해서 사용한다.
- Content의 길이를 알 수 있을 때 사용한다.
- 1번의 요청, 1번에 다 받음

</br></br>

### 2. 압축 전송(Content-Encoding)
<img src="https://user-images.githubusercontent.com/84119178/187425953-b7061cc6-5312-42f2-a598-ad307b76e8a5.png" width = "600"></br>
- 서버에서 압축해서 클라이언트에 보내줌
- Content-Encoding을 꼭 붙여줘야 된다.

</br></br>

### 3. 분할 전송(Transfer-Encoding)
<img src = "https://user-images.githubusercontent.com/84119178/188079591-c83473f6-ea0b-41c1-9bd0-72732bfbd599.png" width = "600"></br>
- 서버에서 분할해서 클라이언트에 보내줌
- Content-Length를 보내면 안된다.
    - Content-Length가 예상이 안가기 때문에

</br></br>

### 4. 범위 전송(Content-Range)
<img src = "https://user-images.githubusercontent.com/84119178/188080930-b30a94d2-90ba-41b3-ad9d-93e27d060d67.png" width = "600"></br>
- 자료를 받다가 중단된 경우 유용하게 사용가능
- 범위를 지정해서 전송할 수 있다.
</br>
<div style="text-align: right"> 

[맨위로](#목차) 
</div>
</br></br>

## 일반 정보
### From
유저 에이전트의 이메일 정보
- 일반적으로 잘 사용되지 않는다.
- 요청에서 사용
</br></br>

### Referer
이전 웹 페이지 주소
- 현재 요청된 페이지의 이전 웹 페이지 주소
- A->B로 이동하는 경우 B를 요청할 때 `Referer:A`를 포함해서 요청
- 유입 경로를 분석할 때 주로 사용
- 요청에서 사용
</br></br>

### User-Agent
유저 에이전트 애플리케이션 정보
- 클라이언트 애플리케이션 정보
- 통계 정보를 뽑기 좋음
- 어떤 브라우저에서 장애 발생하는지 파악이 가능
- 요청에서 사용
</br></br>

### Server
요청을 처리하는 ORIGIN 서버의 소프트웨어 정보
- 응답에서 사용

</br></br>

## 특별한 정보
### Host
요청한 호스트 정보
- **필수 헤더!**
- 하나의 서버/IP주소에 여러 도메인이 적용되어 있을 때,</br>
요청할 도메인이 무엇인지를 나타내주는 헤더
- 요청에서 사용
</br></br>

### Location
페이지 리다이렉션
- 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면,그 위치로 자동 이동한다(리다이렉트)
- 201(Created) : Location 값은 요청에 의해 생성된 리소스 URI
- 3xx : Location 값은 요청을 자동으로 리디렉션하기 위한 대상 리소스를 가리킴
</br></br>

### Retry-After
다음 요청을 하기까지 기다려야 하는 시간
- 503(Service Unavailable) : 서비스가 언제까지 불능인지 알려줄 수 있음
</br></br>

### 인증
1. Authorization
    - 클라이언트 인증 정보를 서버에 전달
    </br></br>
2. WWW-Authenticate
    - 리소스 접근시 필요한 인증 방법 정의
    - 401오류시 무조건 사용
</br>
<div style="text-align: right"> 

[맨위로](#목차) 
</div>
</br></br>

## 쿠키
HTTP는 무상태 프로토콜</br>
-> 클라이언트와 서버는 서로 상태를 유지하지 않는다</br>
-> 그래서 정보를 계속 기억하지 못한다</br>
-> 그래서 쿠키가 필요하다

<img src="https://user-images.githubusercontent.com/84119178/188088593-112f134c-1c03-4876-9d4f-7ca4991275f3.png" width = "600">

**Set-Cookie** : 서버에서 클라이언트로 쿠키 전달(응답)</br>
**Cookie** : 클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP 요청시 서버로 전달</br>

- 사용처
    - 사용자 로그인 세션 관리
    - 광고 정보 트래킹
- 항상 모든 쿠키 정보가 전송됨
    - 네트워크 트래픽 추가 유발(단점)
    - 최소한의 정보만 사용하는 것이 좋음(세션 id, 인증 토큰)
    - 서버에 전송하지 않고, 웹 브라우저 내부에 데이터를 저장하고 싶으면</br>
    웹스토리지를 사용하면 됨
    - 보안에 민감한 데이터는 저장하면 안된다!(주민번호 등등)
---
</br>

- 예) **`set-cookie`** : **sessionId**=abcde1234; **expires**=Sat, 26-Dec-2020 00:00:00 GMT; **path**=/; **domain**=.google.com; **Secure**
    - ### 쿠키 - 생명주기(Expires, max-age)</br>
        - Set-Cookie : **expires**=Sat, 26-Dec-2020 00:00:00 GMT
            - 만료일이 되면 쿠키 삭제
        - Set-Cookie : **max-age**=3600(초)
            - 0이나 음수를 지정하면 쿠키 삭제
        - **`세션 쿠키`** : 만료 날짜를 생략하면 브라우저 종료시 까지만 유지
        - **`영속 쿠키`** : 만료 날짜를 입력하면 해당 날짜까지 유지
        ---
    - ### 쿠키 - 도메인(Domain)
        - **domain=example.org**
        - **명시 : 명시한 문서 기준 도메인 + 서브 도메인 포함**
            - domain=example.org를 지정해서 쿠키 생성
                - example.org는 물론이고</br> 
                하위 도메인(dev.example.org)도 쿠키접근 가능
        - **생략 : 현재 문서 기준 도메인만 적용**
            - example.org에서 쿠키를 생성</br>
                (domain 지정을 생략)
                - example.org에서만 쿠키 접근 가능
                - 하위 도메인은 쿠키 미접근
        ---
    - ### 쿠키 - 경로(Path)
        - **path=/home**
        - 이 경로를 포함한 하위 경로 페이지만 쿠키 접근
        - 일반적으로 path=/ 루트로 지정
            - `/home`(가능)
            - `/home/level1`(가능)
            - `/hello`(불가능)
        ---
    - ### 쿠키 - 보안(Secure, HttpOnly, SameSite)
        - **Secure**
            - 쿠키는 http,https를 구분하지 않고 전송하지만</br>
            Secure를 적용하면 https인 경우에만 전송
        - **HttpOnly**
            - 자바스크립트에서 접근 X
            - HTTP 전송에서만 사용</br>
            (아직 쓰는 곳이 많지 않다!!!)
        - **SameSite**
            - XSRF 공격 방지
            - 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송
</br>
<div style="text-align: right"> 

[맨위로](#목차) 
</div>
