# RESTful API
우선 RESTful API를 알기 위해선 API를 알아야한다.</br>
API라는 개념은 [API란?](https://github.com/crupy/TIL/blob/master/%EA%B8%B0%EC%B4%88%EC%A7%80%EC%8B%9D/API%EB%9E%80%3F.md) 여기 따로 정리해놓았으니 API를 모른다면 보고 오면 도움이 될 것이다.</br>

</br></br>

## RESTful API란?
**REST**란 **RE**presentational **S**tate **T**ransfer의 약어로 웹을 이용할 때 제약 조건들을 정의하는 소프트웨어 아키텍처 스타일이다.</br>
즉, **HTTP를 잘 활용하기 위한 원칙**이라고 말할 수 있으며,</br>
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

1. 인터페이스 일관성 : 일관적인 인터페이스로 분리되어야 한다.
2. 무상태 : 각 요청 간 클라이언트의 context, 세션과 같은 상태 정보를 서버에 저장하지 않는다.
3. 캐시 처리 가능 : 클라이언트는 응답을 캐싱할 수 있어야 하고,</br> 캐시를 통해 대량의 요청을 효율적으로 처리할 수 있다.
4. 계층화 : 클라이언트는 대상 서버에 직접 연결되어있는지, Proxy를 통해서 연결되었는지 알 수가 없다.
5. Code on demand : 자바 애플릿이나 자바스크립트의 제공을 통해 서버가 클라이언트를 실행시킬 수 있는 로직을 전송하여 기능을 확장시킬 수 있다.
6. 클라이언트/서버 구조 : 아키텍처를 단순화시키고 작은 단위로 분리함으로써 클라이언트-서버의 각 파트가 독립적으로 구분하고 서로 간의 의존성을 줄인다.

</br></br>

## REST 구성요소

REST는 다음과 같은 3가지로 구성되어 있다.

1. 자원(Resource) : HTTP URL
2. 자원에 대한 행위 : HTTP Method
3. 자원에 대한 표현(Representations)

</br></br>

## REST API 설계규칙 및 예시

### 1. 소문자를 사용한다.

`❌ https://github.com/crupy/Some-Comments`

`⭕ https://github.com/crupy/some-comments`

- 대문자는 문제를 일으키는 경우가 있기 때문에 소문자로 작성하는 것이 원칙.
</br></br>

### 2. 언더바( _ ) 대신 하이픈( - )을 사용한다.

`❌ https://github.com/crupy/some_comments`

`⭕ https://github.com/crupy/some-comments`

- 정확한 의미나 단어 결합이 불가피한 경우 하이픈("-")을 사용하며 하이픈("-") 사용도 최소한으로 설계한다.
- 언더바("_")는 사용하지 않는다.
</br></br>

### 3. 마지막에 슬래시(/)를 포함하지 않는다.


`❌ https://github.com/crupy/`

`⭕ https://github.com/crupy`

- 슬래시("/")는 계층관계를 나타낼 때 사용한다.
</br></br>

### 4. 행위를 포함하지 않는다.


**`❌POST`** `https://github.com/crupy/post/8`

**`⭕DELETE`** `https://github.com/crupy/8`

- 자원에 대한 행위는 HTTP Method로 표현(GET, POST, DELETE, PUT)
</br></br>

### 5. 파일 확장자는 URL에 포함시키지 않는다.


`❌ https://github.com/crupy/photo.jpg`

`⭕ https://github.com/crupy/photo`</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `HTTP/1.1 HOST: github.com Accept: image/jpg`

- URL이 메시지 body 내용의 포맷을 나타내기 위한 파일 확장자를 적지 않는다.
- 대신 Accept Header를 사용한다.
</br></br>

### 6. 자원에는 형용사, 동사가 아닌 명사를 사용하며, 컨트롤 자원을 의미하는 경우 예외적으로 동사를 사용한다.
- URL은 자원을 표현하는데 중점을 두기 때문에 동사, 형용사보다는 명사를 사용하여야 한다.

</br></br></br>

### 참고
[https://cocoon1787.tistory.com/540](https://cocoon1787.tistory.com/540)
