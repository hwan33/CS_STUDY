# 3 Way Handshake

### 여기서 알아야하는 TCP/IP의 중요한 성질
  - TCP/IP는 **신뢰성**과 **정확성**을 가장 우선으로 한다.
  - 이러한 **신뢰성**과 **정확성**을 보장하기위해, 연결에 대한 논리적인 약속을 설립한다.
  - 연결 데이터를 

### 3 Way Handshake는 한 문장으로
  > TCP는 장치/프로그램들 사이에 논리적인 접속을 성립하기 위해 3 Way Handhshake를 사용한다.

## 기본 개념
  * TCP/IP 프로토콜을 이용하는 응용프로그램이 데이터를 전송하기 전에 **신뢰성**과 **정확성**을 보장하기 위해 3 Way Handshake으로 약속을 수립한다.
  * **SYN**: Synchronize Sequence Numbers의 약자로 **연결 확립 요청**을 의미한다.
  * **ACK**: Acknowledgement의 약자로 **연결 응답 응답**을 의미한다.
  
## 3 Way Handshake의 과정

<img width="623" alt="Screen Shot 2021-10-07 at 11 21 05 PM" src="https://user-images.githubusercontent.com/33091784/136404085-9bad914c-4619-4a8b-ade1-809d383c8b8d.png">

### Step 1

패킷 내용

<img width="603" alt="Screen Shot 2021-10-07 at 11 51 28 PM" src="https://user-images.githubusercontent.com/33091784/136409627-9a272256-c972-41b7-a779-83f6e60a7741.png">

  - A 클라이언트에서 B 서버에게 **SYN 패킷** 을 보낸다.
  - 이 단계에서
  - A 클라이언트는 **SYN/ACK**를 기다리는 **SYN_SENT** 상태
  - B 서버는 **Wait for Client** 상태

### Step 2

패킷 내용

<img width="603" alt="Screen Shot 2021-10-07 at 11 52 08 PM" src="https://user-images.githubusercontent.com/33091784/136409740-be8c0f54-f1ef-45a2-ba5a-b0171629292d.png">

  - B 서버는 **SYN** 요청을 받고 A 클라이언트에게 <요청을 수락한다는 ACK 와 SYN flag> 로 구성된 패킷을 발송하고,
 **ACK** 응답을 기다리며 B서버는 **SYN_RECEIVED** 상태가 된다.

### Step 3

패킷 내용

<img width="603" alt="Screen Shot 2021-10-07 at 11 57 18 PM" src="https://user-images.githubusercontent.com/33091784/136410496-fc76294a-4b5d-40e3-8f8b-f944fb5f83b1.png">

  - A 클라이언트는 B 서버에게 **ACK**를 보내고, 그 후부터 연결이 이루어지고 데이터 전송이 시작된다.
  - 이때 B의 상태는 **Established**이다.

# 4 Way Handshake
  > 4 Way Handshake 는 

## 참고
- 모두의 네트워크<미즈구치 크츠야>
- https://bangu4.tistory.com/74
- https://bangu4.tistory.com/74
- https://kyunghwa-yoo.github.io/tcp-ip-네트워크-스택-이해하기-요약
  


