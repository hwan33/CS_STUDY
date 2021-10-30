# Mutex vs Lock

## Lock과 Mutex의 관계

- **Mutex**는 **lock**이 가능한 객체
- **lock**은 그걸 유지하는 객체. 하드웨어 기반

# Counting Semaphore vs Binary Semaphore

## Counting Semaphore

- `구조체`로 사용됨
    - 세마포어 변수: 0~N 사이의 숫자
        - 임계영역에 접근이 가능한 프로세스/스레드를 의미한다.
- 자원을 **사용**하면 세마포가 **감소**, **방출**하면 세마포가 **증가** 한다.
- 임계구역에 한 번에 여러 프로세스/스레드가 접근할 수 있게 한다.
    - 즉, `상호배제(Mutual Exclusion)` 이 보장 안된다.
    - 반면에, 같은 이유로 `한정대기(Bounded Waiting)`는 보장된다.
        - 왜냐하면, 큐로 프로세스/스레드들이 저장되어있고, 순서대로 임계구역에 들어갈 수 있는 기회가 주어진다.
- `Binary Semaphore` 와는 다르게 정해져있지 않은 범위의 `counter` 개수를 갖는다

### Counting Semaphore 예시 코드
```c
typedef struct {      
        int semaphore_variable;
        Queue list;    //태스크(스레드)들을 저장하는 큐
}counting_semaphore;
```

## Binary Semaphore

- Int 변수로 사용됨.
- 말그대로 0 과 1 두가지 값만 가능하다
- 0이면 임계영역에 접근하고 있는 쓰레드가 있는 상태이고 (busy)
- 1이면 임계영역에 접근하고 있는 쓰레드가 없는 상태이다 (wait)
- 0, 1 을 사용해서 임계구역에 프로세스/쓰레드가 하나씩만 접근할 수 있게 해준다
    - 그래서 `상호배제(Mutual Exclusion)` 가 **보장된다.**
    - 반면에, 프로세스/스레드가 임계구역에 머물 수 있는 시간은 정해져있지 않기 때문에, `한정 대기(Bounded Waiting)`는 **보장 안된다**.
    - 그래서 기아 현상을 초래할 수 있다.
- **이진 세마포어와 Mutex와의 차이점?**
    - 본질적으로 동작하는 방식에 차이점이 조금 있다. (아래에 있는 세마포어 vs 뮤택스 참고)
    - Mutex는 소유하고있는 쓰레드가 직접 release되어야하고(즉, lock을 사용함)
    - 이진 세마포는 그냥 어떤 쓰레드던 signal 을 통해 제어를 할 수 있다.
    - **세마포어는 뮤택스로 사용 될 수 있는데, 반대로 뮤택스는 세마포어로 사용 될 수 없다**
    - 
### Binary Sempahore 예시 코드

```c
typedef struct {
      int semaphore_variable;
}binary_semaphore;
```

# Semaphore vs Mutex

### Mutex

- 상호배제(Mutual Exclustion) 으로 동작하는 `Lock.`

### Semaphore

- 소프트웨어 측면에서 임계구역 문제를 해결을 하려고하는 동기화 도구.
- 뮤텍스보다 조금 더 정교함.
- 뮤텍스와는 다르게 프로세스들간의 signal이 가능해서 프로세스 실행 순서를 조금 더 효율적으로 제어를 할 수 있다.
- 뮤텍스는 너무 대기가 많다.

# 참고

[https://www.geeksforgeeks.org/difference-between-counting-and-binary-semaphores/](https://www.geeksforgeeks.org/difference-between-counting-and-binary-semaphores/)
