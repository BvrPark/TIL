> 이 글은 김영한님의 [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard) 강의의 내용을 바탕으로 정리했습니다.

# HTTP 헤더 - 캐시
### 목차

---
## 캐시
<img src="https://user-images.githubusercontent.com/84119178/188115765-d790f452-65af-43fc-865e-2fee50eacd03.png" width="600"></br>
- 인터넷 네트워크는 매우 느리고 비싸지만 캐시가 없는 경우에는 </br>
데이터가 변경되지 않아도 요청이 올 때마다 네트워크를 통해서 </br>
데이터를 다시 다운로드 받아야 한다.

</br></br>

<img src="https://user-images.githubusercontent.com/84119178/188117714-09fc4e62-662c-4d80-addc-daffedcc3154.png" width="600"></br>
- 캐시를 적용하게 되면 캐시가 유효한 시간동안 HTTP 바디를 다운로드 할 필요없이</br>
HTTP 헤더만 다운로드 받으면 됨
- 네트워크를 통해서 다시 처음부터 다운로드 받을 필요가 없다
- 캐시 유효 시간이 초과하면, 서버를 통해 데이터를 다시 조회하고, 캐시를 갱신</br>
-> 이때 다시 네트워크 다운로드가 발생

</br></br></br>

## 검증 헤더
### **캐시 시간 초과**
- **캐시 만료 후, 서버에서 데이터를 변경하지 않음**
    - 이 경우, 데이터를 전송하는 대신에 저장해 주었던 캐시를 재사용 할 수 있다.
    - 단, 클라이언트의 데이터와 서버의 데이터가 같다는 사실을 확인할 수 있는 방법 필요</br>
    -> **검증헤더**
</br></br>

<img src="https://user-images.githubusercontent.com/84119178/188120595-67948583-36a0-45c9-890a-f36992ff3f7b.png" width = "450">
<img src="https://user-images.githubusercontent.com/84119178/188121208-2613965a-4216-43f5-adfd-218fff89ffb2.png" width = "450">

- 캐시 유효 시간이 초과해도, 서버의 데이터가 갱신되지 않으면</br>
304 Not Modified + 헤더 메타 정보만 응답(HTTP 바디 X)
- 클라이언트는 서버가 보낸 응답 헤더 정보로 캐시의 메타 정보를 갱신</br>
캐시에 저장되어 있는 데이터 재활용
- 용량이 적은 헤더 정보만 다운로드

</br></br></br>

## 조건부 요청
### **검증 헤더**
- 캐시 데이터와 서버 데이터가 같은지 검증하는 데이터
- Last-Modified, ETag
</br></br>

### **조건부 요청 헤더**
- `if-Modified-Since` : Last-Modified 사용
- `if-None-Match` : ETag 사용
- 조건이 만족하면 200 OK</br>
조건이 만족하지 않으면 304 Not Modified</br>
</br></br>

### if-Modified-Since 이후에 데이터 수정시
- 데이터 미변경시
    - 캐시 : 2020-11-10 10:00:00</br>
    서버 : 2020-11-10 10:00:00
    - **304 Not Modified**, 헤더 데이터만 전송(BODY X)
    - 전송 용량이 낮음
    </br></br>
- 데이터 변경시
    - 캐시 : 2020-11-10 10:00:00</br>
    서버 : 2020-11-10 11:00:00</br>
    - **200 OK**, 모든 데이터 전송(BODY O)
    - 전송 용량이 큼
    </br></br>
- ❗❗❗ **if-Modified-Since**는 변경됐냐를 묻는 것이기 때문에, 변경시, **200 OK**가 뜬다.
