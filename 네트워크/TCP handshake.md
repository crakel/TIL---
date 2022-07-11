# TCP handshake

## 3-Way handshake?

`3-Way handshake`는 TCP/IP 프로토콜로 통신하기 전, 정확한 정보 전송을 위해 상대방 컴퓨터와 세션을 연결하는 과정

(TCP 연결 초기화)

<img src="https://ars.els-cdn.com/content/image/3-s2.0-B9781597499613000030-f03-08-9781597499613.jpg">

## 포트(PORT) 상태 정보
- `CLOSED` : 포트가 닫힌 상태
- `LISTEN` : 포트가 열린 상태로 연결 요청 대기 중
- `SYN_RCV` : SYNC 요청을 받고 상대방의 응답을 기다리는 중
- `ESTABLISHED` : 포트 연결 상태

## 플래그 정보
TCP Header에는 CONTROL BIT(플래그비트, 6bit)가 존재

`"URG-ACK-PSH-RST-SYN-FIN"`

* `SYN(Synchronization)` : 연결 요청, 세션을 설정하는데 사용, 초기에 시퀀스 번호(랜덤)를 보냄
* `ACK(Acknowledgement)` : 보낸 시퀀스 번호에 TCP 께층에서의 길이 또는 양을 더한 것과 같은 값을 ACK에 포함하여 전송
  - Client의 시퀀스 번호 + 1 을 ACK로 응답
* `FIN(Finish)` : 연결 해제. 세션 연결을 종료시킬 때 사용되며, 더 이상 전송할 데이터가 없음을 의미


## 3-Way handshake 과정

1. `Client→Server` || 접속을 요청하는 SYN 패킷 전송
    - 송신자가 최초로 데이터를 전송할 때 Sequence Number를 임의의 랜덤 숫자로 지정하고, SYN 플래그 비트를 1로 설정한 세그먼트를 전송한다.
    - PORT 상태
        - Client : CLOSED - SYN_SENT 로 변함
        - Server : LISTEN

2. `Server→Client` || 요청을 수락하는 ACK를 포함한 SYN+ACK 패킷 전송
   - 접속요청을 받은 Q가 요청을 수락했으며, 접속 요청 프로세스인 P도 포트를 열어달라는 메세지를 전송 (SYN-ACK signal bits set)
    - ACK Number필드를 Sequence Number + 1 로 지정하고 SYN과 ACK 플래그 비트를 1로 설정한 새그먼트 전송 (Seq=y, Ack=x+1, SYN, ACK)
    - PORT 상태
        - Client : CLOSED
        - Server : SYN_RCV

3. `Client→Server` || 요청 수신후 다시 ACK를 발송함으로써 연결이 이루어짐
    - 마지막으로 접속 요청 프로세스 P가 수락 확인을 보내 연결을 맺음 (ACK)
    - 이때, 전송할 데이터가 있으면 이 단계에서 데이터를 전송할 수 있다.
    - PORT 상태
        - Client : ESTABLISED
        - Server : SYN_RCV ⇒ ACK ⇒ ESTABLISED


## 4-Way handshake?

`4-Way handshake`는 연결을 해제하는 과정. 여기서 FIN 플래그를 이용

연결 종료에서, 우리가 일반적으로 생각하는 Client와 Server 모두 연결 종료 요청을 할 수 있기때문에 요청자를 Client, 수신자를 Server쪽으로 생각한다.

## Termination의 종류
- `Graceful connection release(정상적인 연결 해제)`

정상적인 연결 해제에서는 양쪽이 커넥션이 서로 모두 커넥션을 닫을 때까지 연결되어 있다.

 - `Abrupt connection release(갑작스런 연결 해제)`

1. 갑자기 한 TCP 엔티티가 연결을 강제로 닫는 경우
2. 한 사용자가 두 데이터 전송 방향을 모두 닫는 경우

## 4-Way handshake 과정
<img src="https://t1.daumcdn.net/cfile/tistory/2152353F52F1C02835">

1. `Client→Server` || 연결을 종료하겠다는 FIN(+ACK)플래그 전송
    - 송신자가 최초로 데이터를 전송할 때 Sequence Number를 임의의 랜덤 숫자로 지정하고, SYN 플래그 비트를 1로 설정한 세그먼트를 전송한다.
  
2. `Server→Client` || Server는 FIN을 받고, ACK를 Client에게 보내고 자신의 통신이 끝날때까지 대기(TIME_WAIT)
   - Server(수신자)는 ACK Number 필드를 (Sequence Number + 1)로 지정하고, ACK 플래그 비트를 1로 설정한 세그먼트를 전송한다.
    - Server는 Client에게 응답을 보내고 CLOSE_WAIT 상태 돌입. 아직 남은 데이터가 있다면 마저 전송을 마친 후에 close( )를 호출
    - Client는 Server에서 ACK를 받은 후에 Server가 남은 데이터 처리를 끝내고 FIN 패킷을 보낼 때까지 기다리게 됩니다. (FIN_WAIT_2)

3. `Server→Client` || 연결이 종료되었다고 FIN플래그 전송
    - 데이터를 모두 보냈다면, 서버는 연결이 종료에 합의 한다는 의미로 FIN 패킷을 클라이언트에게 보낸 후에, 승인 번호를 보내줄 때까지 기다니는 LAST_ACK 상태로 들어간다.

4. `Client→Server` || FIN을 받고, 확인했다는 ACK 전송
    - Client는 FIN을 받고, 확인했다는 ACK를 Server에게 보낸다.
    - 아직 Server로부터 받지 못한 데이터가 있을 수 있으므로 TIME_WAIT을 통해 기다린다. (실질적인 종료과정 CLOSED에 들어가게 된다.)
    - 이때 TIME_WAIT 상태는 의도치 않은 에러로 인해 연결이 데드락으로 빠지는 것을 방지
    - 만약 에러로 인해 종료가 지연되다가 타임이 초과되면 CLOSED로 돌입

- Server는 ACK를 받은 이후 소켓을 닫는다 (Closed)
- TIME_WAIT 시간이 끝나면 Client도 닫는다 (Closed)

## TCP의 특성
- `point-to-point` : 하나의 송신 측과 하나의 수신 측이 통신하는 1:1 통신이다.
- `reliable` : 신뢰성 있는 데이터 전송을 보장한다.
- `pipelined` : TCP 흐름 제어와 혼잡 제어가 window size를 설정한다.
- `full duplex(전이중 통신)` : 쌍방향 통신이 가능하다. 즉 데이터를 주고받을 수 있다.
- `connection-oriented` : 연결 지향적이다. 송신 측과 수신 측이 데이터를 교환하기 전에 handshaking을 한다.
- `flow control` : 흐름 제어를 한다.
- `congestion control` : 혼잡 제어를 한다.
  
## Full-duplex 통신 구성

3-Way handshake에서,


Step 1, 2에서는 P→Q 방향에 대한 연결 파라미터(시퀀스 번호)를 설정하고 이를 승인한다.

Step 2, 3에서는 Q→P 방향에 대한 연결 파라미터(시퀀스 번호)를 설정하고 이를 승인한다.

⇒ 이를 통해 full-duplex 통신이 구축됩니다.


## 참고 

https://velog.io/@averycode/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-TCPUDP%EC%99%80-3-Way-Handshake4-Way-Handshake

https://mindnet.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-22%ED%8E%B8-TCP-3-WayHandshake-4-WayHandshake



