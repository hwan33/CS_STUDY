# 📌들어가기 앞서

## 인터넷 프로토콜 스위트(Internet Protocol Suite) 

<br>

응용 프로그램(호스트)이 인터넷에서 데이터를 주고받기 
위해 사용하는 모든 통신규약(프로토콜) 모음

<br>

<img src="https://user-images.githubusercontent.com/48471292/136427481-775c12b9-f6fc-486f-a65c-1f6a1f3bddce.JPG" width="250px" height="260px" title="Github_Logo"/>

<br><br>

## TCP(Transmission Control Protocol)
인터넷 프로토콜 스위트의 핵심 프로토콜이다. **네트워크 계층의 IP와 함께 TCP/IP라고도 불린다.**

- 특징
  - TCP를 사용함으로써, 호스트들은 안정적으로, 순서대로 데이터를 주고받을 수 있다. 즉, **신뢰성 있는 데이터 전송을 지원하는 연결 지향적 프로토콜이다.**
  - OSI 7계층 중 **전송 계층(Transport Layer**)에서 사용된다.
  - 흐름제어, 혼잡제어, 오류제어를 통해 신뢰성을 보장한다. 하지만 **이러한 장점 때문에 UDP를 사용할 때 보다 속도가 느리다는 단점**이 있다.

<br>

## 프로토콜 데이터 단위(Protocol Data Unit, PDU)
**각 계층 혹은 프로토콜에서 주고 받는 데이터의 단위를 뜻한다.** 데이터 자체는 동일하지만, 각 계층에서 헤더가 추가되면서 이름이 달라진다.

**(하지만 편의상 혼용해서 사용하기도 하는데, 이 글에서도 데이터, 패킷, 세그먼트를 섞어서 사용하겠습니다.)**

<br>

<img src="https://user-images.githubusercontent.com/48471292/136427353-f4b69b54-1952-4460-bb8b-373f5b91a864.png" width="320px" height="320px" title="Github_Logo"/>

<br><br>

# 📌TCP 흐름제어 & 혼잡제어
데이터 통신의 신뢰성을 보장하기 위해 사용하는 기법들이다.

<br>

## 1. 흐름 제어(Flow Control)
수신자의 데이터 처리량과 그에 따른 수신 버퍼의 수용 가능한 용량에 맞춰 데이터를 전송하는 것을 말한다. 즉, **송수신간 데이터 처리 속도를 맞추는 것** 

> 💡 수신자는 버퍼의 남은 용량 정보를 TCP Header에 포함하여 응답한다.

<br>

### 👉 흐름 제어가 왜 필요할까?
수신자의 버퍼 용량 이상으로 데이터를 전송할 경우 **넘치는 데이터는 유실되기에, 불필요한 ACK과 데이터의 재전송이 빈번하게 발생한다.** 그에 따라 통신 속도는 저하되고, 해당 네트워크 구간의 전체 통신량 또한 증가할 것이다.

<br>

### 1) Stop and Wait(정지 - 대기)
- 패킷마다 확인 응답(ACK)를 받아야만 다음 패킷을 전송하는 흐름제어 방법
- 패킷마다 대기하는 구조이기 때문에 **간단하지만 비효율적이다.**

<br>

### 2) Sliding Window(슬라이딩 윈도우)
- Stop and Wait를 보완한 방법
- 수신자가 설정한 window size만큼은 전송자가 응답 여부에 상관없이 패킷을 연속적으로 보낼 수있다.
   
> 💡 **Window)** 모든 Application program은 통신을 하기 위해 socket을 열고, 이곳에 **임시 저장 공간**인 send(송신) buffer와 receive(수신) buffer를 생성한다. **Window는 이 buffer를 의미한다.** <br><br>
    - **send buffer:** 소켓에서 내려온 데이터를 전송하고 ACK을 받기 전까지 저장하는 공간<br>
    - **receive buffer:** 패킷을 순서대로 위 계층으로 올려보내기 위해, 먼저 들어온 패킷을 저장하는 공간

<br>

#### Sliding Window의 동작 방식)
전송자는 세그먼트를 보내고 ACK을 받으면 window를 옆으로 옮기면서(해당 세그먼트를 전송 buffer에서 폐기) 다음 세그먼트를 전송한다. 

![253F7E485715ED5F27](https://user-images.githubusercontent.com/48471292/136476039-046cd068-c91c-4b4e-9f76-2d09bdd8a2db.png)


> 💡 수신자는 1번째 세그먼트를 받으면, 2번째 세그먼트를 전송해달라는 의미로 ACK 2로 응답한다.

<br><br>

## 2. 혼잡 제어(Congestion Control)
- **혼잡이란?** 데이터는 인터넷 네트워크망을 통해 전달되는데, 특정 라우터에 데이터가 과도하게 몰릴 경우 저장 용량을 초과하여 데이터 유실이 발생한다. 이러한 현상을 혼잡이라고 한다.
- 혼잡 현상을 방지하거나 제거하는 것을 혼잡 제어라고 한다.
- 흐름 제어는 송수신자 간의 통신 속도를 다루는 데에 비해, **혼잡 제어는 네트워크 내 라우터를 포함하여 더 넓은 관점에서 이루어진다.**
- 일반적으로 Sliding Window를 사용한다.  

<br>

### 📎 혼잡 발생의 판단 기준

<br>

#### 1) 3 ACK Duplicated
수신자로부터 같은 세그먼트를 요청하는 ACK이 3번 이상 들어올 경우, 전송자는 해당 데이터부터 문제가 발생했다고 판단하고 재전송한다. <br>

- 특징)
  - 해당 세그먼트에 대한 Timeout이 발생하기 전에 문제가 발생했음을 빠르게 판단할 수 있다.
  > 💡 타임아웃 이전에 빠르게 재전송하는 기법을 빠른 재전송(Fast Transmit)이라고 한다.

<br>

![fast-transmit](https://user-images.githubusercontent.com/48471292/136481556-56b0f8d9-5a6e-4eb4-b07a-effb715fde71.png)
(출처: https://evan-moon.github.io/2019/11/26/tcp-congestion-control)

> 💡 TCP는 기본적으로 연속으로 들어온 데이터 중 가장 마지막 데이터에 대한 응답을 보내주는 누적 승인 방식(Cumulative Acknowledgement)을 사용한다.

> 💡 문제의 세그먼트 재전송 후, 수신자는 오류 제어 방식에 따라 어떤 데이터를 요청할지 결정한다.

<br>

#### 2) Timeout
전송자는 세그먼트마다 타이머를 걸어놓고 전송을 하는데, timeout이 될 때까지 ACK이 오지 않을 경우 혼잡 상황이라고 판단한다.

<br>

### 📎 혼잡 회피 기법
<br>

#### 1) AIMD(Additive Increase / Multiplicative Decrease, 합 증가/곱 감소)

Window 내 패킷을 모두 성공적으로 전송했으면 window size를 1 증가시키고, 실패했을 경우 size를 절반으로 감소시키는 기법

<br>

![AIMD](https://user-images.githubusercontent.com/48471292/136478153-a1689ec5-05a7-4c1f-9a91-0d5207ce1cc2.JPG)

- **특징)** AIMD 알고리즘을 사용하는 네트워크에서, 처음 진입하는 호스트는 window size가 작기 때문에 불리하지만, 시간이 흐르면서 모두 비슷한 size를 갖는 평형 상태로 수렴하게 된다.

<br>

- **단점)** 평형 상태로 수렴하기까지 시간이 걸린다.

<br>

#### 2) Slow Start(느린 시작)
AIMD를 개선한 것으로, 패킷들을 모두 성공적으로 보냈을 때 **패킷 개수만큼** window size를 증가시키며, 혼잡 상황이 발생했을 때는 window size를 1로 감소시킨다.

<br>

- **특징)**
  - **AIMD는 window size가 선형적으로 증가하며, Slow Start는 지수적으로 증가한다.** <br>
    > ex) **window size:** 1 -> 2 -> 4 -> 8 -> ...
  
<br>

### 📎 혼잡 제어 정책
수많은 TCP 혼잡 제어 정책이 있지만 그 중, 가장 대표적으로, 초기 버전인 **Tahoe**와 **Reno**가 있다.

<br>

#### 1) TCP Tahoe(타호)
Slow Start를 사용하는 혼잡 제어 정책의 초기 버전으로, Fast Transmission이 처음으로 도입된 정책이다.

<br>

![tahoe-ssthresh](https://user-images.githubusercontent.com/48471292/136483592-80a9b1e2-46cc-402a-b2b6-54d1a0721ff6.png)
(출처: https://evan-moon.github.io/2019/11/26/tcp-congestion-control)

<br>

- 특징)
  - window size를 지수적으로 빠르게 증가시키다가, threshold(임계점) 이후부터는 AIMD와 같이 선형적으로 증가시킨다.
  - 혼잡이 발생했다고 판단하면, window size를 1로 줄이고, 임계점을 절반과 같이 더 작게 수정한다. -> **맞으면서 실전으로 배우는 방식**

- 단점)
  - 윈도우 사이즈를 1로 줄이기 때문에 초반에 크기를 키우는 과정이 오래 걸린다.
  
<br>

#### 2) TCP Reno
Tahoe가 개선된 버전으로, Tahoe와 마찬가지로 slow start로 시작해서 임계점에 도달한 이후에는 합증가 방식을 사용한다.

![reno-ssthresh](https://user-images.githubusercontent.com/48471292/136484576-3655f7e7-e908-462c-b094-a94b3adb2a1c.png)
(출처: https://evan-moon.github.io/2019/11/26/tcp-congestion-control)

- 특징)
  - 3 ACK Duplicated와 Timeout를 구분해서 window size를 수정한다.
  - 3 ACK Duplicated 발생 -> window size 절반 감소, threshold는 줄어든 윈도우로 재설정
    > 💡 window size를 1로 줄이는 것에 비해 빠르게 원래 size로 돌아갈 수 있어서 **Fast Recovery**(빠른 회복)라고 부른다.
  - Timeout 발생 -> window size = 1, threshold 그대로
  - 즉, ACK 중복은 Timeout에 비해 큰 혼잡이 아니라고 생각하고, 상황에 따라 대처한다.

<br>

# 📌참고
- 전체 흐름)
  - gyoogle: https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Network/TCP%20(%ED%9D%90%EB%A6%84%EC%A0%9C%EC%96%B4%ED%98%BC%EC%9E%A1%EC%A0%9C%EC%96%B4).md
  - 이석복 교수님 강의: http://www.kocw.net/home/cview.do?cid=e2c0e0696048d484
  - 로키 산맥님 블로그: https://rok93.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-TCP-%ED%9D%90%EB%A6%84%EC%A0%9C%EC%96%B4%ED%98%BC%EC%9E%A1%EC%A0%9C%EC%96%B4
- PDU)
  - daehyunlee님 블로그: https://velog.io/@hidaehyunlee/%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%8A%B8-%ED%8C%A8%ED%82%B7-%ED%97%B7%EA%B0%88%EB%A6%B4-%EB%95%90-PDU%EB%A5%BC-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90

- 흐름제어) 
  - 개발자를 꿈꾸는님 블로그: https://jwprogramming.tistory.com/36
- 혼잡제어)
  - Evans님 블로그: **https://evan-moon.github.io/2019/11/26/tcp-congestion-control/**
  - Jeff-Kang님 블로그: https://rain-bow.tistory.com/entry/TCP-congestion-control


