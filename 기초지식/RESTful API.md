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
</br></br>

## REST 특징

### REST 아키텍처에 적용되는 6가지 제한 조건

- 인터페이스 일관성 : 일관적인 인터페이스로 분리되어야 합니다.
- 무상태 : 각 요청 간 클라이언트의 context, 세션과 같은 상태 정보를 서버에 저장하지 않습니다.
- 캐시 처리 가능 : 클라이언트는 응답을 캐싱할 수 있어야 합니다. 캐시를 통해 대량의 요청을 효율적으로 처리할 수 있습니다.
- 계층화 : 클라이언트는 대상 서버에 직접 연결되어있는지, Proxy를 통해서 연결되었는지 알 수 없습니다.
- Code on demand : 자바 애플릿이나 자바스크립트의 제공을 통해 서버가 클라이언트를 실행시킬 수 있는 로직을 전송하여 기능을 확장시킬 수 있습니다.
- 클라이언트/서버 구조 : 아키텍처를 단순화시키고 작은 단위로 분리함으로써 클라이언트-서버의 각 파트가 독립적으로 구분하고 서로 간의 의존성을 줄입니다.
