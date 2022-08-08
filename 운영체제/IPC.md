# IPC

## IPC?
Inter Process Communication

: 프로세스 간 통신

프로세스들끼리 서로 데이터를 주고받는 행위 또는 그에 대한 방법을 뜻한다.

프로세스는 원래 독립적이지만, 상황에 따라 프로세스끼리 협력해야 되는 경우가 발생한다.

이때 커널이 제공하는 IPC라는 설비를 이용해 프로세스간 통신을 할 수 있게 된다.

## IPC가 필요한 상황
### 정보공유
여러 사용자가 동일한 정보에 액세스 할 필요성
### 가속화
특정 작업을 여러 서브 작업으로 쪼개 병렬성 향상을 통한 처리속도를 높일 필요성
### 모듈화
특정 시스템의 기능을 별도의 프로세스로 구분하여 모듈화 시스템 구성 필요성
### 편의성 
다수의 사용자가 동시에 여러 작업을 수행할 필요성
## IPC의 종류
<img src="https://velog.velcdn.com/images%2Fyanghl98%2Fpost%2F9d620bb0-54a2-48bd-b30f-d4f0fc4d0303%2Fimage.png">

### 메세지 전달 (Message Passing)
커널을 통해 메세지를 전달하는 방식으로 자원과 데이터 통신

- Pipe, Signal, Message Queueing, Socket
#### 장점
커널이 제공하는 API를 이용하기 때문에 구현이 쉬움
#### 단점
시스템 콜이 필요해 오버헤드 발생
### 공유 메모리 (Shared Memory)
<img src="https://mblogthumb-phinf.pstatic.net/MjAxOTEwMDhfMjg1/MDAxNTcwNDg1MDQ3ODU3.e1IfDq0VdnXD_SGZnpcHzF7EQBnTaIml5V_fp1XrH7Ag.mwTfBhrpBFeLFf387IkobsZH6zGJ1ogZqGB6y8ZBaNgg.PNG.demonic3540/image.png?type=w800">

공유 메모리 영역을 구축하고 이 영역을 통해 자원과 데이터 통신

공유 메모리는 커널에서 관리된다.

공유 메모리를 사용하기 위해서는 다음조건을 만족해야한다.

- 메모리 보호 제약 해제
- 동시에 동일한 위치를 쓰지 않도록 프로세스들이 책임을 져야함

- Semaphores
#### 장점
커널 의존도가 낮아 속도가 빠르고 통신이 자유로움
#### 단점
공유 영역을 사용하기 때문에 동기화 이슈 발생 


## 파이프(Pipe & Named Pipe)
- 선입선출 형태로 구성된 메모리를 여러 프로페스가 공유하여 통신 수행
- 하나의 프로세스가 파이프를 통해 다른 프로세스를 직접 전달하며 데이터는 한 쪽 방향으로만 이동
- 하나의 프로세스가 Pipe를 이용중이면 다른 프로세스는 접근 불가

## 시그널(Signal)
- PID를 통해 특정 프로세스에 메세지 전달

## 메시지 큐잉(Message Queueing)
- 메시지가 발생하면 고정 크기의 메시지를 연결리스트를 통해 프로세스 간 통신 수행

## 소켓 (Socket)
- 네트워크 상에서 프로세스 간에 통신 하는 방식
- 원격 통신도 가능

## 세마포어 (Semaphore)
- 의미는 공유되고 있는 자원의 개수(변수) (0, 1, n)
- 동기화 문제를 해결하기 위해 사용됨, 여러 프로세스 사이에서 동작의 순서를 지정
- 더 자세한 내용은 Semaphore와 Mutex를 비교하면서..


