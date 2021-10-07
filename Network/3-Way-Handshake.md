# 3 Way Handshake

### 여기서 알아야하는 TCP/IP의 중요한 성질
  - TCP 는 Connection-Oriented (연결 지향) 프로토콜이다.
  - TCP/IP는 **신뢰성**과 **정확성**을 가장 우선으로 한다.
  - 이러한 **신뢰성**과 **정확성**을 보장하기위해, 연결에 대한 논리적인 약속을 설립한다.
  - 연결 데이터를 

### 3 Way Handshake/4 Way Handshake 의 패킷 구성

<img width="604" alt="Screen Shot 2021-10-08 at 12 24 56 AM" src="https://user-images.githubusercontent.com/33091784/136415319-69114d0a-23d7-43ab-b510-b8da7ea8a8cf.png">
  - **URG**: URGENT 포인터란 전송하는 데이터 중에서 긴급히 전달해야 할 내용이 있을 경우에 사용한다. 긴급한 데이터는 다른 데이터에 비해 우선순위가 높아야한다.
   EX) ping 명령어 실행 도중 Ctrl+c 입력
  - **ACK**: ACKNOWLEDGEMENT의 약자로, 상대방으로부터 패킷을 받았다는 걸 알려주는 패킷, 다른 플래그와 같이 출력되는 경우도 있습니다.
받는 사람이 보낸 사람 시퀀스 번호에 TCP 계층에서 길이 또는 데이터 양을 더한 것과 같은 ACK를 보냅니다.(일반적으로 +1 하여 보냄) ACK 응답을 통해 보낸 패킷에 대한 성공, 실패를 판단하여 재전송 하거나 다음 패킷을 전송한다.
  - **PSH**: PUSH 란, 밀어넣기, ELNET 과 같은 상호작용이 중요한 프로토콜의 경우 빠른 응답이 중요한데, 이 때 받은 데이터를 즉시 목적지인 OSI 7 Layer 의 Application 계층으로 전송하도록 하는 FLAG. 대화형 트랙픽에 사용되는 것으로 버퍼가 채워지기를 기다리지 않고 데이터를 전달한다. 데이터는 버퍼링 없이 바로 위 계층이 아닌 7 계층의 응용프로그램으로 바로 전달한다.
  - **RST**: RESET란, 재설정(Reset)을 하는 과정이며 양방향에서 동시에 일어나는 중단 작업이다. 비 정상적인 세션 연결 끊기에 해당한다. 이 패킷을 보내는 곳이 현재 접속하고 있는 곳과 즉시 연결을 끊고자 할 때 사용한다.
  - **SYN**: 

출처: https://mindgear.tistory.com/206 [정리]

출처: https://mindgear.tistory.com/206 [정리]

출처: https://mindgear.tistory.com/206 [정리]

출처: https://mindgear.tistory.com/206 [정리]


### 3 Way Handshake는 한 문장으로
  > TCP는 장치/프로그램들 사이에 접속 세션을 성립하기 위해 3 Way Handhshake를 사용한다.

## 기본 개념
  * TCP/IP 프로토콜을 이용하는 응용프로그램이 데이터를 전송하기 전에 **신뢰성**과 **정확성**을 보장하기 위해 3 Way Handshake으로 약속을 수립한다.
  * **SYN**: Synchronize Sequence Numbers의 약자로 **연결 확립 요청**을 의미한다.
  * **ACK**: Acknowledgement의 약자로 **연결 응답 응답**을 의미한다.
  
## 3 Way Handshake의 과정

<img width="623" alt="Screen Shot 2021-10-07 at 11 21 05 PM" src="https://user-images.githubusercontent.com/33091784/136404085-9bad914c-4619-4a8b-ade1-809d383c8b8d.png">

### Step 1

  - A 클라이언트에서 B 서버에게 **SYN** 을 보낸다.
  - 이 단계에서
  - A 클라이언트는 **SYN/ACK**를 기다리는 **SYN_SENT** 상태
  - B 서버는 **Wait for Client** 상태

패킷 내용

<img width="603" alt="Screen Shot 2021-10-07 at 11 51 28 PM" src="https://user-images.githubusercontent.com/33091784/136409627-9a272256-c972-41b7-a779-83f6e60a7741.png">

### Step 2

  - B 서버는 **SYN** 요청을 받고 A 클라이언트에게 <요청을 수락한다는 ACK 와 SYN flag> 로 구성된 패킷을 발송하고,
 **ACK** 응답을 기다리며 B서버는 **SYN_RECEIVED** 상태가 된다.
 
패킷 내용

<img width="603" alt="Screen Shot 2021-10-07 at 11 52 08 PM" src="https://user-images.githubusercontent.com/33091784/136409740-be8c0f54-f1ef-45a2-ba5a-b0171629292d.png">


### Step 3

  - A 클라이언트는 B 서버에게 **ACK**를 보내고, 그 후부터 연결이 이루어지고 데이터 전송이 시작된다.
  - 이때 B의 상태는 **Established**이다.

패킷 내용

<img width="603" alt="Screen Shot 2021-10-07 at 11 57 18 PM" src="https://user-images.githubusercontent.com/33091784/136410496-fc76294a-4b5d-40e3-8f8b-f944fb5f83b1.png">

# 4 Way Handshake

### 4 Way Handshake는 한 문장으로
  > 4 Way Handshake 는 3 Way Handshake로 성립했던 연결 세션을 끊기 위해 하는 절차이다.

## 기본 개념
  - 이 과정을 통해, server, client 는 tcp 연결이 해제되며 연결을 위해 사용했던 resource를 정리한다.

## 4 Way Handshake의 과정

<img width="603" alt="Screen Shot 2021-10-08 at 12 01 12 AM" src="https://user-images.githubusercontent.com/33091784/136411270-a88098c5-f642-4e06-8218-3b12e2db22c9.png">

### Step 1
  - A 클라이언트가 연결을 종료하겠다는 **FIN** 플래그를 B 서버에게 전송한다
  - 이때, A 클라이언트는 **FIN_WAIT** 상태가 된다.
  
  패킷 내용
  
  <img width="603" alt="Screen Shot 2021-10-08 at 12 08 51 AM" src="https://user-images.githubusercontent.com/33091784/136412703-9320be5c-4a6b-401c-be3e-9f228f38fd14.png">

### Step 2
  - B 서버는 **FIN** 플래그를 받고, 
  - 일단 확인 응답 **ACK** 를 보내고 자신의 통신이 끝날때까지 기다리는데 
  - 이때 B 서버의 상태는 CLOSE_WAIT 상태이다.
  
  패킷 내용
  
  <img width="603" alt="Screen Shot 2021-10-08 at 12 15 24 AM" src="https://user-images.githubusercontent.com/33091784/136413742-1939a957-de8a-4726-a50d-b0890b66df20.png">

### Step 3
  - 연결을 종료하기 위해, 연결해지를 위한 준비가 되었음을 알리기 위해 A 클라이언트에게 **FIN** 플래그를 전송한다.
  - 이때 B의 상태는 **LAST_ACK** 이다
  
  패킷 내용
  
  <img width="603" alt="Screen Shot 2021-10-08 at 12 08 51 AM" src="https://user-images.githubusercontent.com/33091784/136412703-9320be5c-4a6b-401c-be3e-9f228f38fd14.png">
  
### Step 4
  - 클라이언트는 해지 준비가 되었다는 **ACK** 를 보낸다.
  - 이때 A 클라이언트의 상태가 **FIN_WAIT** 에서 **TIME_WAIT** 으로 변경된다.
  
  패킷 내용
  
  <img width="603" alt="Screen Shot 2021-10-08 at 12 15 24 AM" src="https://user-images.githubusercontent.com/33091784/136413742-1939a957-de8a-4726-a50d-b0890b66df20.png">

## 참고
- 모두의 네트워크<미즈구치 크츠야>
- https://bangu4.tistory.com/74
- https://bangu4.tistory.com/74
- https://kyunghwa-yoo.github.io/tcp-ip-네트워크-스택-이해하기-요약
- https://mindgear.tistory.com/206
  


