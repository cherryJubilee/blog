---
draft: false
authors:
  - hyewon
date: 2024-01-06
categories:
  - Study
  - Web
---
# 3 way handshake, 4 way handshake
<!-- more -->


## TCP 란
Transmission Control Protocol = 연결형 통신 프로토콜. (신뢰성 > 효율성)

handshaking 과정을 통해서 신뢰성을 보장한다.  

UDP 에 비해 더 정확한 정보를 정확하지만, UDP에 비해 통신 속도가 느리다.  

TCP에 의해 연결 확립, 재전송 제어, 윈도우 제어가 일어난다.  

SMTP, HTTP, HTTPS, FPS 등 우리에게 친숙한 인터넷 서비스 대부분이 TCP/IP로 이뤄져 있다. TCP/IP 에서 IP는 데이터 순서, 손실에 고려하지 않는 반면, TCP는 느리지만 신뢰도가 높다.  

---
## 3-Way handshake

3-Way handshake는TCP/IP 프로토콜로 통신하기 전 정확한 정보 전송을 위해 상대방 컴퓨터과 세션을 연결/수립 하는 과정입니다. (=TCP 연결 초기화)
클라이언트가 서버에게 접속을 요청하는 SYN신호을 보내면, 서버는 요청을 수락하는 ACK를 포함하여 SYN+ACK신호를 클라이언트에게 발송합니다. 그러면 클라이언트가 이것을 수신한 후 다시 ACK를 서버에게 발송하면 연결이 이루어지고, 이로써 데이터를 주고받을 수 있게 됩니다.

``` mermaid
sequenceDiagram
    participant Client
    participant Server

    Client->>Server: SYN: 연결해줘
    note right of Server: 서버는 클라이언트의 <br/> 연결 요청을 받음
    Server->>Client: SYN + ACK
    note left of Client: 서버: 요청 받았어(ACK) <br/> 나도 연결해줘(SYN)
    Client->>Server: ACK
    note right of Server: 클라이언트: 요청 확인했어
```

---

## 4-Way handshake


3-Way handshake를 통해 Connection Setup을 했다면, TCP연결을 종료하는 Connection Termination 과정은 4-Way handshake를 통해 이루어집니다.

1. Client 에서 Process 종료할 떄 FIN 신호를 보낸다.
2. Server는 FIN 신호를 받았다는 응답에 대한 ACK를 Client로 보냅니다. 단, 서버에서 클라이언트로 데이터 전송이 완료가 아직 안 된 상태일 수 있습니다.
3. 서버에서 클라이언트로 모든 데이터 전송이 완료된 다음, FIN 신호를 클라이언트로 보냅니다.
4. 서버가 클라이언트로부터 ACK를 받으면 TCP 연결이 종료됩니다.

``` mermaid
sequenceDiagram
    participant 클라이언트
    participant 서버

    클라이언트->>서버: FIN: 내가 보낼 건 다 보냈어, 연결 끊자
    서버->>클라이언트: ACK: 알겠어, 남은 패킷 보내줄게
    Note over 서버: 서버가 남은 데이터를 전송 완료
    서버->>클라이언트: FIN: 나도 보낼 거 다 보냈어, 이제 끝내자
    클라이언트->>서버: ACK: 알겠어
    Note over 클라이언트,서버: TCP 연결 종료 완료

```


> 연결을 시작하려고 할 때는 = 아무런 데이터 전달이 없는 상태  
> SYN -> ACK/SYN -> ACK  
> 연결을 끊으려고 할 때는 = 데이터가 전송중인 상태일 수 있다  
> FIN -> ACK -> 하지만 네이버에서 혜원이로 아직 데이터가 전송중이야 -> 전송이 끝나면 -> FIN -> ACK  
