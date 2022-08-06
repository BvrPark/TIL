> 이 글은 김영한님의 [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard) 강의의 내용을 바탕으로 정리했습니다.

# URI(Uniform Resource Identifier)
<img src = "https://user-images.githubusercontent.com/84119178/183239596-1baeac04-a5ff-4207-808a-3f8e9339f210.png" width = "600" height = "365">
</br></br>

## 뜻
**U**niform : 리소스를 식별하는 통일된 방식</br>
**R**esource : 자원, URI로 식별할 수 있는 모든 것</br>
**I**dentifier : 다른 항목과 구분하는데 필요한 정보</br>

- UR**L** - Locator : 리소스가 있는 위치를 지정
- UR**N** - Name : 리소스에 이름을 부여

URN 이름만으로 실제 리소스를 찾을 수 있는 방법이 보편화 되지 않음.</br>
일반적으로 URL을 더 많이 사용한다.
</br></br>


## URL 문법
`scheme://[userinfo@]host[:port][/path][?query][#fragment]`</br>
`https://www.google.com:443/search?q=hello&hl=ko`</br>
- [프로토콜(https)](#scheme)
- [호스트명(www.google.com)](#host)
- [포트 번호(443)](#port)
- [경로(/search)](#path)
- [쿼리 파라미터(q=hello&hl=ko)](#query)

### **scheme**
**`scheme:`**`//[userinfo@]host[:port][/path][?query][#fragment]`</br>
**`https:`**`//www.google.com:443/search?q=hello&hl=ko`</br>
- 주로 프로토콜을 사용하는 자리
- 프로토콜 : 어떤 방식으로 자원에 접근할 것인가 하는 약속 규칙
    -   ex) http, https, ftp 등등
- http는 80포트, https는 433포트를 주로 사용하고 포트는 생략 가능
</br></br>

### **userinfo**
`scheme://`**`[userinfo@]`**`host[:port][/path][?query][#fragment]`</br>
`https://www.google.com:443/search?q=hello&hl=ko`</br>
- URL에 사용자 정보를 포함해서 인증
- 거의 사용하지 않는다.
</br></br>

### **host**
`scheme://[userinfo@]`**`host`**`[:port][/path][?query][#fragment]`</br>
`https://`**`www.google.com:443`**`/search?q=hello&hl=ko`</br>
- 호스트명
- 도메인명(DNS)이나 IP 주소를 직접 사용가능
</br></br>

### **PORT**
`scheme://[userinfo@]host`**`[:port]`**`[/path][?query][#fragment]`</br>
`https://www.google.com`**`:443`**`/search?q=hello&hl=ko`</br>
- 포트(PORT)
- 접속포트, 일반적으로 생략해서 쓰고 생략시 http는 80, https는 443 사용
</br></br>

### **path**
`scheme://[userinfo@]host[:port]`**`[/path]`**`[?query][#fragment]`</br>
`https://www.google.com:443`**`/search`**`?q=hello&hl=ko`</br>
- 리소스 경로(path), 계층적 구조를 가짐
- ex)
    - /folder/1.jpg
    - /home/100, /items/iphone13
</br></br>

### **query**
`scheme://[userinfo@]host[:port][/path]`**`[?query]`**`[#fragment]`</br>
`https://www.google.com:443/search`**`?q=hello&hl=ko`**</br>
- 여러 이름으로 불리며(query string, parameter) 웹서버에서 제공하는 파라미터
- key = value의 형태를 가지고 있으며 ?로 시작, &로 추가가 가능하다
- ? keyA = valueA & keyB = valueB</br>
    ❗❗**가독성을 위하여 여백을 줬지만 원래는 여백 없이 사용**
- 문자를 쓰든 숫자를 쓰든, 항상 String의 형태로 전달된다
</br></br>

### **fragment**
`scheme://[userinfo@]host[:port][/path][?query]`**`[#fragment]`**</br>
`https://docs.spring.io/spring-boot/docs/current/reference/html/gettingstarted.
html`**`#getting-started-introducing-spring-boot`**</br>
- fragment
- html 내부 북마크 등에 사용된다.
- 서버에 전송하는 정보가 아니다.
