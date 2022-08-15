> 이 글은 김영한님의 [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard) 강의의 내용을 바탕으로 정리했습니다.

# HTTP 메서드
- [GET](#get)
- [POST](#post)
- [PUT](#put)
- [PATCH](#patch)
- [DELETE](#delete)
- [메서드의 속성](#http-메서드의-속성)

</br></br>
<img src = "https://user-images.githubusercontent.com/84119178/184474003-c65ea962-8997-4b12-949e-0fa5f91dbc99.png" width = "300">

보통 URI를 설계할 때, 이런식으로 설계를 하지만 URI는 리소스만 식별을 해야한다.</br>
여기서 리소스는 회원 등록, 조회같은 행위가 아닌 **회원이라는 개념 자체**가 리소스가 된다.</br>

<img src = "https://user-images.githubusercontent.com/84119178/184474195-7278fdc7-12c6-412a-90a2-4d4fe3134d6a.png" width = "500">

따라서 이런 식으로 URI를 설계해주는 것이 좋다.</br>
**URI는 리소스만 식별해주면 된다!**</br>
리소스가 아닌 행위에 해당되는 조회, 등록, 삭제, 변경은 HTTP메서드로 구분한다.

</br></br>

## HTTP 메서드 종류

### 주요 메서드
- **GET** : 리소스 조회
- **POST** : 요청 데이터를 처리(주로 등록에 사용)
- **PUT** : 리소스를 대체해준다.</br>
만약, 해당 리소스가 없으면 새로 생성해준다. 
- **PATCH** : 리소스를 부분적으로 변경
- **DELETE** : 리소스를 삭제

### 기타 메서드
- **HEAD** : GET과 기능은 동일하지만, 바디를 제외한 헤더까지의 부분만 반환을 해준다.
- **OPTIONS** : 대상 리소스에 대한 통신 가능 메서드를 설명(주로 CORS에서 사용한다.)
</br></br></br>

## GET
- 리소스 조회
- 서버에 전달하고 싶은 데이터는 query를 통해서 전달한다.
- 메시지 바디를 통해서도 데이터를 전달할 수 있지만, 권장하지 않는다.

조회 과정
<img src = "https://user-images.githubusercontent.com/84119178/184474674-b3399ed7-2f2c-429f-a05a-2dfaa8fa6442.png" width = "600"> 
<img src = "https://user-images.githubusercontent.com/84119178/184474704-ac9f3ea3-162e-48f4-86b9-90142f1512b7.png" width = "600">

</br></br></br>

## POST
- **클라이언트가 메시지 바디를 통해 서버로 요청 데이터 전달**
- 메시지 바디를 통해 들어온 데이터를 처리하는 모든 기능을 수행.
- 주로 리소스 등록, 프로세스 처리에 사용.

<img src = "https://user-images.githubusercontent.com/84119178/184475030-1cfa34a9-2d29-472e-a80b-96b6ef78b362.png" width = "500">
<img src = "https://user-images.githubusercontent.com/84119178/184475050-8d26441d-44da-4bde-8311-9251ed55145d.png" width = "550">
<img src = "https://user-images.githubusercontent.com/84119178/184475076-7780ba89-1d02-40e3-a810-5694514153b8.png" width = "550">
<img src = "https://user-images.githubusercontent.com/84119178/184475098-7b74037f-b7d2-43f0-adb3-e72d563e9724.png" width = "600">

</br></br></br>

## PUT
- 리소스가 있으면 대체(덮어쓰기)
- 리소스가 없으면 생성
- **클라이언트가 리소스를 식별!!!**
    - 클라이언트가 리소스 위치를 알고 URI 지정</br>
    -> POST와 PUT의 차이점

### 리소스가 존재
<img src = "https://user-images.githubusercontent.com/84119178/184475574-723c4c8b-5f9d-44e4-83e9-169db3430eae.png" width = "500">
<img src = "https://user-images.githubusercontent.com/84119178/184475624-00d7963b-712a-4192-8b03-2c6299164adf.png" width = "550">
</br></br>

### **주의할 점!**
<img src = "https://user-images.githubusercontent.com/84119178/184475747-ac8eec16-f9a7-4496-b9ae-92fae5cf1cad.png" width = "500">
<img src = "https://user-images.githubusercontent.com/84119178/184475775-539493fe-6b12-4334-bf61-ebe73dd76fae.png" width = "550">
</br></br>

### 리소스가 존재X
<img src = "https://user-images.githubusercontent.com/84119178/184475671-04ca59c6-f95a-4974-a30d-5bd463873c5a.png" width = "500">
<img src = "https://user-images.githubusercontent.com/84119178/184475693-fa4e8619-d89f-4bdb-b644-27a7223dcbb9.png" width = "550">

</br></br></br>

## PATCH
- 리소스 부분 변경
- PATCH가 안되는 경우는 POST를 사용!

<img src = "https://user-images.githubusercontent.com/84119178/184475905-9aa4df4c-d75c-4768-b1e9-50c1251c557f.png" width = "500">
<img src = "https://user-images.githubusercontent.com/84119178/184475937-27e93660-4764-4e33-8272-f28e7ff423f2.png" width = "500">

</br></br></br>

## DELETE
- 리소스 제거

<img src = "https://user-images.githubusercontent.com/84119178/184475986-534e3fb0-ef4d-42b2-b814-5a599569841e.png" width = "500">
<img src = "https://user-images.githubusercontent.com/84119178/184476005-ed74393f-5e7a-483e-a406-edb2cdcf0ca3.png" width = "500">

</br></br></br></br>

## HTTP 메서드의 속성

### 1. 안전(Safe Methods)
호출해도 리소스를 변경하지 않는다.(GET, HEAD 등등)</br>
데이터가 쌓여서 장애가 발생하는 그런 부분까지는 고려하지 않는다.

### 2. 멱등(Idempotent Methods)
1번이든 100번이든 호출한 결과가 항상 똑같다.</br>
-> 서버가 정상 응답을 못 주었을 때, 클라이언트가 같은 요청을 다시 해도 되는가?에 대한 판단의 근거가 된다.

- 멱등 메서드
    - GET : 1번 조회하든, 2번 조회하든 같은 결과가 조회.

    - PUT : 결과를 대체한다.</br> 
    -> 같은 요청을 여러 번 해도 최종 결과는 같다.

    - DELETE : 결과를 삭제한다.</br> 
    -> 같은 요청을 여러 번 해도 삭제된 결과는 똑같다.

    - **POST : 멱등이 아니다!!!**</br>
    2번 호출하면 같은 결제가 중복해서 발생할 수도 있다.
   

- **멱등은 외부 요인으로 중간에 리소스가 변경되는 것 까지는 고려하지 않는다.**


### 3. 캐시가능(Cacheable Methods)
**`캐시`** : 웹 브라우저가 자신의 로컬 PC에 어떤 데이터를(이미지, 영상 등) 저장하고 있는 것

- GET, HEAD, POST, PATCH를 캐시로 사용가능
- 실제로는 GET, HEAD 정도만 캐시로 사용
    - POST, PATCH는 본문 내용까지 캐시 키로 고려해야 하는데, 구현이 쉽지 않다.


## 데이터 전송 예시
<img src = "https://user-images.githubusercontent.com/84119178/184593909-4cb0b7bb-b319-40a7-a044-8b0cda773405.png" width = "600">
