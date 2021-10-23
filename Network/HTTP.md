# 📌HTTP란)
인터넷에서 데이터를 주고받기 위한 **Text** 기반의 **응용 계층** 프로토콜로, 클라이언트/서버 모델을 따른다.

<br>

# 📌TCP/IP vs HTTP

## 1. 동작 계층
**TCP/IP** -> 전송 계층, **HTTP** -> 응용 계층

즉, **HTTP는 전송 계층 위에서 동작하며,** 그 위에서 어떤 방식으로 웹에서 동작 할지를 정한다. 따라서, **전송 계층의 관점에서 본다면, HTTP는 전송 프로토콜의(ex: TCP/IP)의 특징을 모두 갖고 있다.** 
> 💡 HTTP는 신뢰 가능한 전송 프로토콜이라면 이론상으로 UDP나 TCP, 무엇이든 사용할 수 있으나, 주로
TCP 혹은 암호화된 TCP 연결인 TLS를 통해 전송한다.

<br>

<img width="798" alt="HTTP   layers" src="https://user-images.githubusercontent.com/48471292/138263679-1ef4fbfa-4a99-4128-a08c-bbe0d5c89034.png">

(출처: https://developer.mozilla.org/ko/docs/Web/HTTP/Overview)

<br>

## 2. 데이터 형식
- **TCP/IP**: Byte Array
- **HTTP**: String(텍스트)

<br>

## 3. 연결 방식
- **TCP/IP**: 연결 지향적
- **HTTP**: 비연결, **Stateless**라고 함


    <br> 👉 **HTTP가 비연결적이다?**
    
    연결은 전송 계층에서 제어되므로 HTTP의 영역 밖이다. 따라서, **응용 계층의 관점에서 볼 때 HTTP는 비연결적 프로토콜이다.**

    (연결성은 아래에서 더 자세하게 다루겠습니다.)

<br>

# HTTP 흐름
1. 클라이언트-서버간 TCP 연결을 한다. 
2. **요청**: 클라이언트가 HTTP 메시지를 전송한다.
3. **응답**: 서버가 요청에 따른 응답 메세지를 보낸다.
4. 연결을 닫거나 다른 요청을 위해 연결을 재사용한다.

<br>

# HTTP 메시지

## 요청(Request)
![HTTP_Request](https://user-images.githubusercontent.com/48471292/138268530-cf537675-fcee-411c-8345-6020a804e217.png)
(출처: https://developer.mozilla.org/ko/docs/Web/HTTP/Overview)

<br>

클라이언트가 서버에게 리소스나 작업을 요청하는 것이다. **시작줄**, **헤더**, **본문**으로 구성된다.

<br>

### 1. 시작줄(Request line)
[메소드] [리소스 경로(ex: URL)] [HTTP 버전] 으로 구성된다.

  #### HTTP 메소드)
  - 클라이언트가 어떤 동작을 요청하는지 알려주는 역할을 한다.
  - **GET(가져오다), POST(게시), PUT(전체 수정, 대체), PATCH(부분 수정), DELETE(삭제)가 주로 쓰이며**, OPTIONS, HEAD, CONNECT, TRACE 등이 있다.
  
<br>

### 2. 헤더 
2번째 줄부터 빈 줄 전까지 해당하며, **요청에 대한 추가 정보**를 전달할 수 있다. User-Agent, Upgrade-Insecure-Request를 비롯해서 매우 많은 종류의 정보가 있다.

<br>

### 3. 본문
헤더에서 한 줄 띈 곳부터 시작하여 **실제 전달할 데이터**를 담는 부분이다. 주소로 웹사이트를 요청하는 경우처럼 비어있을 수도 있다.

<br>

## 2. 응답(Response)
  ![HTTP_Response](https://user-images.githubusercontent.com/48471292/138268584-e3266c03-66cc-41ed-aa67-c5d560100a29.png)
  (출처: https://developer.mozilla.org/ko/docs/Web/HTTP/Overview)

  서버가 클라이언트의 요청에 대한 답변을 보내는 것이다. **시작줄**, **헤더**, **본문**으로 구성된다.

<br>

### 1. 시작줄(Response line)
[버전] [상태 코드] [상태 메시지]로 구성된다.

  #### 상태 코드(Status Code):
  **요청의 성공 여부와 이유**를 알려준다. 굉장히 많은 종류가 있으며 세자리 수로 이루어져 있다. 
  
  - 1XX (조건부 응답) : 요청을 받았으며 작업을 계속한다.
  - 2XX (성공) : 클라이언트가 요청한 동작을 수신하여 이해했고 승낙했으며 성공적으로 처리했음을 가리킨다
  - 3XX (리다이렉션) : 클라이언트는 요청을 마치기 위해 추가 동작을 취해야 한다.
  - 4XX (요청 오류) : 클라이언트에 오류가 있음을 나타낸다.
  - 5XX (서버 오류) : 서버가 유효한 요청을 명백하게 수행하지 못했음을 나타낸다.
  
<br>

### 2. 헤더
마찬가지로 응답에 대한 추가 정보를 전달한다.

<br>

### 3. 본문
클라이언트가 요청한 리소스를 전달한다.

<br>

# HTTP 성능과 진화
HTTP는 비연결적 프로토콜로 **HTTP/1.0**의 전통적인 동작은 하나의 요청/응답마다 별도의 TCP를 여는 것이었는데, 매번 새로운 연결로 인한 성능 저하와 서버 부하의 문제가 있었다. 

이에 대한 해결책으로 connection 헤더가 도입되어 tcp 연결의 재활용이 가능해졌다.  

이후 좀 더 근본적인 개선을 위해 **HTTP/1.1**에서 파이프라이닝 개념과 Persistent Connection 개념을 도입되었고, **HTTP/2.0**에서는 단일 연결에서의 다중 전송(Multiplexing)을 비롯해 여러 기능이 추가되면서 더욱 효율적인 통신이 가능해졌다.

그리고 2013년 구글에서 보안, 신뢰성, 속도를 향상시킨 UDP 기반의 전송 프로토콜 QUIC를 발표했다. 그리고 이를 기반으로 한 **HTTP/3.0**이 등장했으며, 현재는 약 전체 웹사이트의 8%가 HTTP/3를 지원하고 있다.

<br>

# 📌참고
1. 전체 개요
   - https://velog.io/@surim014/HTTP%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80
   - https://www.zerocho.com/category/HTTP
   - https://developer.mozilla.org/ko/docs/Web/HTTP
2. HTTP vs TCP/IP
   - https://velog.io/@sa1341/HTTP%EC%99%80-TCP%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90
   - https://hwan-shell.tistory.com/271 
3. HTTP의 진화
   - https://velog.io/@guswns3371/HTTP1.1-HTTP2-QUIC
   - https://study-ihl.tistory.com/171