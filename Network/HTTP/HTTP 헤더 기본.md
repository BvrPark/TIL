> 이 글은 김영한님의 [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard) 강의의 내용을 바탕으로 정리했습니다.

# HTTP 헤더 기본
HTTP 헤더는 다음과 같은 구조를 가지고 있다.

**헤더 필드의 구조(Host: www.google.com)**</br>
`field-name ":" OWS field-value OWS` (OWS:띄어쓰기 허용)</br>
<img src = "https://user-images.githubusercontent.com/84119178/186613927-c2da2854-f8d0-47bd-84c7-887cb45b276e.png" width = "600">

</br></br>

**HTTP BODY**

<img src = "https://user-images.githubusercontent.com/84119178/186615214-464617c1-be50-4aad-b913-88b6ab4aaf05.png" height = "200">

- 메시지 본문(Message body)을 통해 표현 데이터 전달
    - 원래는 엔티티(Entity)라고 쓰였지만 최신버전에서는 표현(Representation)이라고 쓰임
- 메시지 본문 = 페이로드(payload) = 실제 데이터 부분
- 표현(표현 헤더 + 표현 데이터) : 요청이나 응답에서 전달할 실제 데이터

</br></br>

## 표현
<img src = "https://user-images.githubusercontent.com/84119178/186644703-d5941579-8d3e-418e-b75c-76e8d1350b28.png" width = "600">

1. ### Content-Type : 표현 데이터의 형식을 설명
    - 미디어 타입이나 문자 인코딩 상태를 설명
    - ex) text/html; charset=uft-8</br>
        application/json</br>

2. ### Content-Encoding : 표현 데이터 인코딩
    - 표현 데이터를 압축하기 위해 사용된다
    - 데이터를 전달하는 곳에서 압축한 뒤, 인코딩 헤더를 추가한다.
    - 데이터를 읽는 쪽에서 인코딩 헤더의 정보로 압축 해제
3. ### Content-Language : 표현 데이터의 자연언어
    - ex) ko, en, en-US.....
4. ### Content-Length : 표현 데이터의 길이
    - 바이트 단위
    - Transfer-Encoding을 사용할때는 쓰면 안됨!!!
