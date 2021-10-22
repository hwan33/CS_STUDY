# Blocking vs Non-Blocking

- 호출된 함수가 바로 리턴하느냐 마느냐에 대한 이야기

### Blocking

- **호출된 함수**가 할일을 끝내기 전까지 **호출한 함수**에게 제어권을 넘겨주지 않고 대기하게 만드는 것.
- linux 에서 모든 소켓 통신은 기본적으로B**locking Model 으로 동작.**
- **I/O** 작업이 진행되는 동안 유저 프로세스는 자신의 작업을 중단한 채 대기하는 방식.

<img width="655" alt="Screen Shot 2021-10-20 at 9 27 49 PM" src="https://user-images.githubusercontent.com/33091784/138099328-768dbc53-cef4-4819-8b8d-989467a763c8.png">




1. 유저는 커널에게 read 작업을 요청
2. 데이터가 입력될때까지 대기
3. 데이터가 입력되면 유저에게 결과가 전달돼야 유저 자신의 작업을 진행할 수 있다.
- 위와 같이 **Block**이 되면, 다른 작업을 수행하지 못하고 대기하게 되므로 자원이 낭비되는 비효율성이 있다.

### Non-Blocking

- **호출된 함수**가 바로 리턴해서 **호출한 함수**가 다른 일을 할 수 있게 제어권을 넘겨주는것.
- 위의 **Blocking Model** 의 비효율성을 해결하기 위해 도입된 방식.
    - I/O 작업이 진행되는 동안 유저 프로세스의 작업을 중단시키지 않는 방식

<img width="655" alt="Screen Shot 2021-10-20 at 9 32 40 PM" src="https://user-images.githubusercontent.com/33091784/138099313-5c82147c-464f-4b37-9ff2-f488b3c4fc24.png">


1. 유저가 커널에게 read 작업을 요청
2. 데이터 입력의 유무와 상관없이, 요청하는 순간 바로 결과가 반환된다.
    1. 이때 입력 데이터가 없으면 입력 데이터가 없다는 메세지(EWOULDBLOCK)를 반환
3. 입력 데이터가 있을때까지 1~2를 반복.
    1. 2번에서 결과 메세지를 받은 유저는 다른 작업 진행이 가능하다.
4. 입력 데이터가 있으면 유저에게 결과가 전달된다.

- **Non-Blocking Model** 에서 I/O의 진행시간과 관계가 없기 때문에 어플리케이션에서 작업을 오랜 시간 중지하지 않고도 I/O 작업을 진행할 수 있다.
- 그런데 반복적으로 시스템 호출이 발생하기 때문에 이때도 자원 낭비가 발생한다.
- **Non-Blocking 의 문제를 해겨하기 위해 I/O 이벤트 통지 모델이 도입됨 (동기/비동기)**

## I/O 이벤트 모델

- 수신 버퍼나 출력 버퍼의 이벤트를 통지한다는 의미이다.
- 수신 버퍼의 이벤트: 입력 버퍼에 데이터가 수신되었다는 것을 알림
- 출력 버퍼의 이벤트: 출력 버퍼가 비었으니 데이터 전송이 가능한 상황을 알림
- I/O 작업 결과 반환 방식에 따라 **동기/비동기**로 나뉠 수 있다.

# Synchronous vs Asynchronous

- 호출되는 함수의 작업 완료 여부를 누가 신경쓰냐에 대한 이야기

### Synchronous(동기)

- **호출된 함수**의 수행 결과 및 종료를 **호출한 함수**가 신경쓰는 것.
- **호출되는 함수**로부터 바로 리턴 받더라도 작업 완료 여부를 **호출하는 함수** 스스로 계속 확인할때.
- I/O 작업이 진행되는 동안 유저 프로세스는 결과를 기다렸다가 이벤트를 직접 처리하는 방식.
    - 이때 유저 프로세스는 **Blocking** 처럼 다 될때까지 기다릴 수도 있고,
    - **Non-Blocking** 처럼 커널에 계속 요청하는 방식으로 기다릴 수도 있다.

### Aynchronous(비동기)

- **호출된 함수**의 수행 결과 및 종료를 **호출된 함수**가 혼자 직접 신경쓰고 처리하는 것.
- I/O 작업이 진행되는 동안 유저 프로세스는 관심이 없다. 자신의 일을 하다가 이벤트 핸들러에 의해 알림이 오면 처리하는 방
- 유저 프로세스는 요청을 한 후에, I/O 동기화를 신경쓸 필요가 없고 이벤트 핸들러(또는 callback)에 의해 운영체제에서 처리 결과를 통지 받는 방식.
- 결국 커널이 담당하여 주체적으로 notification을 담당하며, 유저 프로세스는 수동적인 입장에서 통지가 오면 그때 I/O 처리를 하면 된다.

![blocking_nonblocking_sync_async](https://user-images.githubusercontent.com/33091784/138099222-2cf4093e-9842-463c-a963-84aed1754b66.png)


## Blocking & Synchronous

<img width="626" alt="Screen Shot 2021-10-20 at 10 51 07 PM" src="https://user-images.githubusercontent.com/33091784/138106139-2db2d5b7-653f-42b2-919d-0ec3e295c67a.png">

- 프로그램이 Blocking을 일으키는 시스템 함수를 호출
- 한 작업당 한 번의 사용자-커널 사이의 문맥교환 발생
- 정지된 프로그램은 CPU를 사용하지 않고 커널의 응답을 대기
- 프로그램 관점에서 보면 마치 처리 로직이 오래걸리는것 같지만, 사실은 커널의 일을 기다리느라 Block 되어 있는 것.

- 예시
    
    나 : 핫 황금올리브 치킨 한마리 주세요
    
    치킨집 사장님 : 넵 알겠습니다 그 자리에 그대로 서계셔서 기다려주세요.
    
    나 : …?!!
    
    치킨집 사장님 : (치킨 만들기)
    
    나 : (과정을 지켜본다.. 다른거 아무것도 안하고 치킨 튀기는거 구경한다)
    

## Blocking & Asynchronous
- 이점이 없어서 사용되는 경우가 없는것 같다.
- 원래는 Non-Blocking & Aync 를 추가하다가 Block-Async가 되어 버리는 경우가 있다고 한다.

- 예시
    
    나 : 핫 황금올리브 치킨 한마리 주세요
    
    치킨집 사장님 : 넵 알겠습니다 그 자리에 그대로 서계셔서 기다려주세요.
    
    나 : …?!!
    
    치킨집 사장님 : (치킨 만들기)
    
    나 : (배가 별로 안고파서 치킨 관심 없는데 다른건 못하고 계속 그자리에 서있다)
    

## Non-Blocking & Synchronous

<img width="626" alt="Screen Shot 2021-10-20 at 10 53 46 PM" src="https://user-images.githubusercontent.com/33091784/138106611-676a157c-5e9e-4715-b156-42b5edce0de7.png">


- Non-Blocking 함수를 호출하고 바로 반환 받아서 다른 작업을 할 수 있게 되지만, 함수 호출에 의해 수행되는 작업이 완료된 것은 아니고, 호출하는 메서드가 호출되는 함수 쪽에 작업 완료 여부를 계속 문의하는 것이다.
- Blocking & Synchronous 의 개선안이지만 비효율적이다.
    - 왜냐하면 Non-Blocking 방식은 정상 데이터가 올때까지 계속 시스템콜을 하며 문맥교환을 한다.
    - Latency를 초래
- 예시
    
    나 : 핫 황금올리브 치킨 한마리 주세요
    
    치킨집 사장님 : 넵 알겠습니다 가서 다른 일 하시다가 다시 와주세요.
    
    나 : 넵. 
    
    치킨집 사장님 : (치킨 만들기)
    
    나 : 치킨 다 됐나요?
    
    치킨집 사장님 : 아직요.
    
    나 : 치킨 다 됐나요?
    
    치킨집 사장님 : 아직요.
    
    나 : 치킨 다 됐나요?
    
    치킨집 사장님 : 아직요.
    

## Non-Blocking & Asynchronous

<img width="626" alt="Screen Shot 2021-10-20 at 10 55 27 PM" src="https://user-images.githubusercontent.com/33091784/138106887-2606a66d-0a87-4fd2-b575-08ce7928a007.png">


- 시스템콜이 즉시 IO개시 여부를 반환.
- 사용자 프로세스는 다른일을 할 수 있고, CPU는 다른 업무를 볼 수 있다.
- IO는 백그라운드에서 처리된다.
- IO 응답이 도착하면 신호나 콜백으로 IO 전달을 완료한다.

- 예시
    
    나 : 핫 황금올리브 치킨 한마리 주세요
    
    치킨집 사장님 :  넵 알겠습니다 가서 다른 일 하시다가 다시 와주세요.
    
    나 : 넵!
    
    치킨집 사장님 : (치킨 만들기)
    
    나 : (핸드폰 한다)
    
    치킨집 사장님 : 치킨 나왔습니다
    
    나 : :)

# Blocking vs Non-Blocking vs Sync vs Async 를 구분하는 다양한 기준들

- Non-Blocking vs Async를 **동작** 관점에서 구분
    - Non Blocking은 제어문 수준에서 지체없이 반환하는 것
    - Aync 는 별도의 쓰레드로 빼서 실행하고, ㄹ완료되면 호출하는 측에 알려주는 것
- **입장** 으로서의 구분
    - Blocking/Non-Blocking은 호출한 입장
    - Sync/Async 는 처리되는 방식의 입장

# 참고

[https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer Science/Network/[Network] Blocking%2CNon-blocking %26 Synchronous%2CAsynchronous.md](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Network/%5BNetwork%5D%20Blocking%2CNon-blocking%20%26%20Synchronous%2CAsynchronous.md)

[http://homoefficio.github.io/2017/02/19/Blocking-NonBlocking-Synchronous-Asynchronous/](http://homoefficio.github.io/2017/02/19/Blocking-NonBlocking-Synchronous-Asynchronous/)

[https://musma.github.io/2019/04/17/blocking-and-synchronous.html](https://musma.github.io/2019/04/17/blocking-and-synchronous.html)

[https://ju3un.github.io/network-basic-1/](https://ju3un.github.io/network-basic-1/)

[https://ju3un.github.io/network-basic-2/](https://ju3un.github.io/network-basic-2/)

https://sjh836.tistory.com/109

https://ozt88.tistory.com/20
