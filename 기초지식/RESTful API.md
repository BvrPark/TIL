# RESTful API
우선 RESTful API를 알기 위해선 API를 알아야한다.</br>
API라는 개념은 [API란?](https://github.com/crupy/TIL/blob/master/%EA%B8%B0%EC%B4%88%EC%A7%80%EC%8B%9D/API%EB%9E%80%3F.md) 여기 따로 정리해놓았으니 API를 모른다면 보고 오면 도움이 될 것이다.</br>
</br></br>

## RESTful API란?
**REST**란 **RE**presentational **S**tate **T**ransfer의 약어로 웹을 이용할 때 제약 조건들을 정의하는 소프트웨어 아키텍처 스타일이다.</br>
`HTTP URL`을 통해 자원(Resource)을 명시하고 `*HTTP Method`를 통해서 해당 자원(URL)에 대한 **CRUD**(**C**reate, **R**ead, **U**pdate, **D**elete)를 적용하는 것을 의미한다.
</br></br>

**HTTP Method**
- GET : 지정된 URL에서 리소스의 표현을 조회
- POST : 지정된 URL에 신규 리소스를 생성
- PUT : 지정된 URL에 리소스를 생성하거나 업데이트
- PATCH : 리소스의 부분 업데이트
- DELETE : 지정된 URL의 리소스를 제거
