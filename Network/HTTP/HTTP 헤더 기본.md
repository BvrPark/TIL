> 이 글은 김영한님의 [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard) 강의의 내용을 바탕으로 정리했습니다.

# HTTP 헤더 기본
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

</br></br></br>

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
    
</br></br></br>

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
