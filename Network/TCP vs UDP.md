> 이 글은 김영한님의 [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard) 강의의 내용을 바탕으로 정리했습니다.

# TCP vs UDP
TCP와 UDP는 OSI 7계층 중에서 `Layer4 : 전송계층`에서 사용되는 프로토콜에 해당됨.</br>
### 인터넷 프로토콜 스택의 4계층
- **애플리케이션 계층** - HTTP, FTP
- **전송 계층** - TCP, UDP
- **인터넷 계층** - IP
- **네트워크 인터페이스 계층**

<img src="https://user-images.githubusercontent.com/84119178/181900806-a14abd13-8dd4-4b5a-8224-eeb77dca5c85.png" width="800" height="480"/>
</br></br>

## TCP : 전송 제어 프로토콜(Transmission Control Protocol)

### 1. 일반적으로 **TCP**와 **IP**가 함께 사용. 
- **IP**는 데이터 전송을 처리, **TCP**는 패킷 추적 및 관리를 담당한다.
<img src="https://user-images.githubusercontent.com/84119178/181902920-b06a0382-5c81-4618-8844-4a70cee2637d.png" width="500" height="300"/>
</br></br>

### 2. 연결지향 : TCP 3-way handShaking(가상연결)
- 연결을 했는지 확인 후, 통신을 시작
<img src="https://user-images.githubusercontent.com/84119178/181903357-3bf1ac7f-a882-4aeb-bfb5-a316310188cb.png" width="500" height="300"/>

- 1->2->3->4의 순서대로 통신을 한다.
    
1) 클라이언트에서 서버로 접속요청(SYN)을 보냄
2) 서버에서 요청을 수락(ACK)함과 동시에 서버도 클라이언트에게 접속을 요청함(SYN)
3) 클라이언트에서 요청을 수락(ACK)
4) 이 과정이 끝난 후, 데이터를 전송
    
- 3,4번 과정은 동시에 일어날 수도 있음.
</br></br>

### 3. 흐름제어와 혼잡제어를 지원하며 데이터의 순서를 보장.
- `흐름제어` : 보내는 측과 받는 측의 데이터 처리속도 차이를 조절해주는 것
- `혼잡제어` : 네트워크 내의 패킷 수가 넘치게 증가하지 않도록 방지하는 것
<img src="https://user-images.githubusercontent.com/84119178/181908569-983112e5-699e-4c7c-a45f-ee5c364daa16.png" width="700" height="300"/>
</br></br>

### 4. 데이터 전달 보증
- 일반적인 인터넷 프로토콜과는 달리 데이터 누락과 같은 문제가 발생했을시, 오류가 발생했다고 보여준다.

</br></br>
### TCP 특징정리
- 연결형 서비스로 가상 회선 방식을 제공
- 데이터의 전송 순서 보장
- 데이터 전달 보증
- 신뢰할 수 있는 프로토콜
- 현재는 대부분 TCP 사용

</br></br></br>

## UDP(User Datagram Protocol)
비연결형 프로토콜.</br> 
인터넷상에서 서로 정보를 주고받을 때 정보를 보낸다는 신호나 받는다는 신호 절차를 거치지 않고 보내는 쪽에서</br> **일방적으로 데이터를 전달**하는 통신 프로토콜.</br> 
TCP와는 다르게 연결 설정이 없으며, 혼잡 제어를 하지 않기 때문에 TCP보다 전송 속도가 빠름.</br> 
데이터 전송에 대한 보장을 하지 않기 때문에 패킷 손실이 발생할 수 있다.
</br></br>

### UDP 특징
- 비연결형 서비스로 데이터그램 방식을 제공
- 비신뢰성
- 데이터의 경계를 구분
- 패킷 오버해드가 적어 네트워크 부하 감소
- 혼잡 제어를 하지 않기 때문에 TCP보다 빠름
- TCP의 handshaking 같은 연결 설정이 없음
- 연속성이 중요한 실시간 스트리밍 같은 서비스에 자주 사용

</br></br></br>

## 요약

![TCP vs UDP](https://user-images.githubusercontent.com/84119178/173500911-c4111ceb-d205-4808-81e2-dce47bf92415.png)
</br>

![TCP,UDP그림](https://user-images.githubusercontent.com/84119178/173501199-1f6b7a74-57c7-49b7-a9a0-f056532ee26b.png)

</br></br></br></br>

### 참고
[https://cocoon1787.tistory.com/757](https://cocoon1787.tistory.com/757)</br>
[모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard)
