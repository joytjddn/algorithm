# [운영체제] IPC(Inter Process Communication)



프로세스는 독립적으로 실행되기도 하고 데이터를 주고받으며 협업하기도 한다. 즉, 프로세스 간 영향을 받지 않는다.(스레드는 프로세스 안에서 자원을 공유하므로 영향을 받는다.) 이러한 프로세스 간 통신을 가능하게 해주는 것이 바로 IPC 통신이다.



#### 프로세스 간 통신 개념

* 프로세스들 사이에 서로 데이터를 주고 받는 행위 또는 그 방법이나 경로

* 네트워크로 연결된 타른 컴퓨터에 있는 프로세스와의 통신도 포함



##### 	종류

	* **프로세스 내부 데이터 통신**: 하나의 프로세스 내에 2개 이상의 스레드가 존재하는 경우의 통신. 프로세스 내부의 스레드는 전역 변수나 파일을 이용.
	* **프로세스 간 데이터 통신** : 같은 컴퓨터에 있는 여러 프로세스끼리 통신하는 경우. PIPE 이용.
 * **네트워크를 이용한 데이터 통신** : 여러 컴퓨터가 네트워크로 연결되어 있을때. 소켓을 이용한 통신(이를 **네트워킹**이라고 한다.)



IPC는 단순하게 **send** 와 **receive**의 개념.

내부적으로는 통신 상태 프로세스 위치, 주고받는 데이터의 크기, 데이터의 도작 여부 확인 등을 체크해야 한다.



#### 프로세스 간 통신 분류



##### 1.통신 방향에 따른 분류

* **양방향 통신** : 데이터를 통시에 양방향으로 전송할 수 있는 구조. ex)소켓 통신
* **반양방향 통신** : 데이터 양방향 전송가능. 단, 동시 전송 불가능. ex)무전기
* **단방향통신** : 한쪽 방향으로만 데이터 전송 ex)전역변수,파일, 파이프
  * 전역변수가 단방향???
    * 데이터의 저장공간이 하나이기 때문에 양쪽에서 보낼 경우 다른 하나의 데이터는 지워진다. 전역변수가 2개인 경우, 양방향 통신 가능. 

##### 2.통신 구현 방식에 따른 분류

**동기, 비동기에 대한 개념** - 데이터를 받는 시점에 대한 이야기. 전역변수를 이용한 통신의 경우, 받는 입장에서 언제 데이터를 보낼 지 알 수 없기 때문에 반복적으로 변수값 변화를 점검해야 한다. 이때, 데이터의 도착을 확인하는 것을 **동기화(synchronization)** 라고 한다.

* 동기화 통신 : 데이터가 도착할 때까지 대기. ex)파이프, 소켓
* 비동기 통신 : 지속적으로 데이터 도착 여부 직접 확인. ex)전역변수, 파일





#### IPC 종류

| **IPC 종류**               | **PIPE**                    | **Named PIPE**                  | **Mesage Queue**                | **Shared Memory**              | **Memory Map**                  | **Socket**                    |
| -------------------------- | --------------------------- | ------------------------------- | ------------------------------- | ------------------------------ | ------------------------------- | ----------------------------- |
| **사용시기**               | 부모 자식 간단 방향 통신 시 | 다른 프로세스와 단 방향 통신 시 | 다른 프로세스와 단 방향 통신 시 | 다른 프로세스와양 방향 통신 시 | 다른 프로세스와 양 방향 통신 시 | 다른 시스템간 양 방향 통신 시 |
| **공유매개체**             | 파일                        | 파일                            | 메모리                          | 메모리                         | 파일+메모리                     | 소켓                          |
| **통신단위**               | Stream                      | Stream                          | 구조체                          | 구조체                         | 페이지                          | Stream                        |
| **통신** **방향**          | 단 방향                     | 당 방향                         | 단 방향                         | 양 방향                        | 양 방향                         | 양 방향                       |
| **통신** **가능** **범위** | 동일 시스템                 | 동일 시스템                     | 동일 시스템                     | 동일 시스템                    | 동일 시스템                     | 동일 + 외부 시스템            |



프로세스는 커널이 제공하는 IPC 설비를 이용해 프로세스간 통신을 할 수 있게 된다.

***커널이란?***

```text
운영체제의 핵심적인 부분으로, 다른 모든 부분에 여러 기본적인 서비스를 제공.
```



1. ##### 익명 PIPE

   파이프는 두 개의 프로세스를 연결하는데 하나의 프로세스는 데이터를 쓰기만 하고, 다른 하나는 데이터를 읽기만 할 수 있다.

   **한쪽 방향으로만 통신이 가능한 반이중 통신**이라고도 부른다.

   따라서 양쪽으로 모두 송/수신을 하고 싶으면 2개의 파이프를 만들어야 한다.

   매우 간단하게 사용할 수 있는 장점이 있고, 단순한 데이터 흐름을 가질 땐 파이프를 사용하는 것이 효율적이다. 단점으로는 전이중 통신을 위해 2개를 만들어야 할 때는 구현이 복잡해지게 된다.

2. ##### Named PIPE(FIFO)

   익명 파이프는 통신할 프로세스를 명확히 알 수 있는 경우에 사용한다. (부모-자식 프로세스 간 통신처럼)

   Named 파이프는 전혀 모르는 상태의 프로세스들 사이 통신에 사용한다.

   즉, 익명 파이프의 확장된 상태로 **부모 프로세스와 무관한 다른 프로세스도 통신이 가능한 것** (통신을 위해 이름있는 파일을 사용)

   하지만, Named 파이프 역시 읽기/쓰기 동시에 불가능함. 따라서 전이중 통신을 위해서는 익명 파이프처럼 2개를 만들어야 가능

3. ##### Message Queue

   입출력 방식은 Named 파이프와 동일.

   다른점은 메시지 큐는 파이프처럼 데이터의 흐름이 아니라 메모리 공간이다.

   사용할 데이터에 번호를 붙이면서 여러 프로세스가 동시에 데이터를 쉽게 다룰 수 있다.

   ![](C:\Users\hw030\OneDrive\문서\GitHub\algorithm\message queue.png)

#### 

#### 출처

* https://doitnow-man.tistory.com/110 [즐거운인생 (실패 또하나의 성공)]

* https://whitecode.tistory.com/67

* 쉽게 배우는 운영체제