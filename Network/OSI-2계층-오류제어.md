
# OSI 2계층에서의 오류제어 방식

## 1. 순방향 FEC(Forward Error Correction)
- **송신측**에서 전송한 데이터와 잉여비트를 통한 오류 제어
- **수신측**에서 오류를 정정하는 방식
- 특징
	- 주요 용도:
    	- 송신측이 한 곳이고 수신측이 여러 곳일때, 재전송/되돌려보내는 피드백이 어려운 곳,
        - 피드백이 어려운 곳, 채널환경이 열악한 곳, 높은 신뢰성이 요구되는 곳에서 필요
        - 실시간 처리 및 높은 처리율 제공
        - 오류가 발생해도 재전송이 필요 없다
    - 장점:
        - 연속적인 데이터 전송이 가능
        - 역채널을 사용하지 않음
    - 단점:
        - 잉여 비트에 의한 전송 채널 대역이 낭비되며
        - 기기와 코드 방식이 복잡
    - FEC 코드의 종류:
        - 블록 코드:
            - 선형 코드의 가장 대표적인 순회 코드(해밍 코드, CRC 코드, BCH 코드)
        - Convolution 코드:
            - 각 블록에서의 부호화가 그 블록뿐만 아니라, 그 이전의 블록에도 의존
                
                하는 부호로 비블록 코드 또는 비선형 코드, 트리 부호라고도 함
                
        - LDPC
    
## 2. 역방향 BEC(Backward Error Correction)
- 오류가 발생했을때 재전송을 요구하는 방식
- 오류 검출 기법:
<img width="646" alt="Screen Shot 2021-11-05 at 8 08 30 PM" src="https://user-images.githubusercontent.com/33091784/140501730-5cfd4918-5895-47b7-b8f0-096928d9fb9f.png">
    
- 오류제어 기법:
<img width="643" alt="Screen Shot 2021-11-05 at 8 10 31 PM" src="https://user-images.githubusercontent.com/33091784/140501902-e9688f9f-3662-493e-9956-2e0f66a9440d.png">

### Go-Back-N ARQ
- Protocol Pipelining 개념
- Sender는 첫 프레임에 대한 ACK를 받기 전까지 여러개의 Frame를 보낼 수 있다
- 정해진 개수의 Frame를 순차적으로 보낸다
- 윈도우 사이즈에 따라 프레임의 순번이 정해진다
- 특정 Frame 에 대한 ACK를 정해진 시간안에 못받으면, 현재 슬라이딩 윈도우 안에 있는 모든 Frame는 재전송된다.
- Go-Back-N 에서 `N` 은 윈도우의 사이즈를 의미한다.

![IMG_0770](https://user-images.githubusercontent.com/33091784/140504786-6ace1fa4-64d3-467f-9bd1-df6a066ff653.jpg)

### Selective ARQ
- 수신자는 제대로 받은(오류가 없는)모든 패킷에 대한 ACK를 보낸다
- 즉, 오류가 발생한 프레임만 재전송된다.
- 송신자는 못 받은 ACK에 대한 패킷만 재전송한다.
- Sender window: 연속된 N개의 seq number
	
![IMG_0771](https://user-images.githubusercontent.com/33091784/140505278-d3acee3f-4541-4595-9eed-e852d2d1e23b.jpg)

# 참고

[http://mystudyroom.net/2021/01/07/문-osi참조모델-2계층에서의-오류제어방식에-대하여-설/](http://mystudyroom.net/2021/01/07/%EB%AC%B8-osi%EC%B0%B8%EC%A1%B0%EB%AA%A8%EB%8D%B8-2%EA%B3%84%EC%B8%B5%EC%97%90%EC%84%9C%EC%9D%98-%EC%98%A4%EB%A5%98%EC%A0%9C%EC%96%B4%EB%B0%A9%EC%8B%9D%EC%97%90-%EB%8C%80%ED%95%98%EC%97%AC-%EC%84%A4/)

[http://www.ktword.co.kr/test/view/view.php?m_temp1=693](http://www.ktword.co.kr/test/view/view.php?m_temp1=693)

[http://blog.skby.net/bec-backward-error-correction/](http://blog.skby.net/bec-backward-error-correction/)

