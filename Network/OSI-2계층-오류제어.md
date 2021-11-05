
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

![IMG_0766](https://user-images.githubusercontent.com/33091784/140503094-dee299c6-45e0-496d-b2af-bd60e18cddad.jpg)
- 이 상태에서 Sender는 다음 Frame인 4를 보낸다
- 그리고 Sliding Window는 왼쪽으로 이동한다

![IMG_0767](https://user-images.githubusercontent.com/33091784/140503379-1ca1cd9d-5637-4c74-a826-ba00a9d18926.jpg)
1. 여기서 Frame 1를 Acknowledge 한다
2. 그 다음 Frame 인 5를 보낸다
3. Sliding Window 가 왼쪽으로 이동한다
4. 만약 ACK를 시간안에 못 받으면 Retransmit

![IMG_0768](https://user-images.githubusercontent.com/33091784/140503625-28a8832f-0aaf-42f3-9f16-f2d66f5fdf78.jpg)
1. 2 ACK를 Timeout 전에 전달받지 못했기 때문에 2~5까지 전부 다시 전송한다(Current Window)

![IMG_0769](https://user-images.githubusercontent.com/33091784/140503797-bc7bef05-27f1-4d0e-b133-aab6c6ec5f93.jpg)
1. 4, 5는 버려진다

# 참고

[http://mystudyroom.net/2021/01/07/문-osi참조모델-2계층에서의-오류제어방식에-대하여-설/](http://mystudyroom.net/2021/01/07/%EB%AC%B8-osi%EC%B0%B8%EC%A1%B0%EB%AA%A8%EB%8D%B8-2%EA%B3%84%EC%B8%B5%EC%97%90%EC%84%9C%EC%9D%98-%EC%98%A4%EB%A5%98%EC%A0%9C%EC%96%B4%EB%B0%A9%EC%8B%9D%EC%97%90-%EB%8C%80%ED%95%98%EC%97%AC-%EC%84%A4/)

[http://www.ktword.co.kr/test/view/view.php?m_temp1=693](http://www.ktword.co.kr/test/view/view.php?m_temp1=693)

[http://blog.skby.net/bec-backward-error-correction/](http://blog.skby.net/bec-backward-error-correction/)

