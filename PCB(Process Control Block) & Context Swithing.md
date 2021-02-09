# PCB(Process Control Block) & Context Swithing



### Process Managemant

> CPU가 프로세스가 여러개일 떄 CPU 스케줄링을 통해 관리하는 것을 말함

CPU는 각 프로세스들이 누군지 알아야 관리가 가능. 프로세스들의 특징을 가지고 있는 정보가 바로 **Process Metadata**

* Process Metadata (프로세스가 생성되면 PCB에 저장)
  * Process ID : 각 프로세스를 구분하는 구분자(메모리에는 여러 개의 프로세스가 존재)
  * Process state : 프로세스 상태에는 생서, 준비, 실행, 대기, 보류 wns비, 보류 대기 등이 있다. 이는 현재 어떤 상태에 있는지 를 나타낸다.
  * Process Priority : 프로세스 우선순위 등과 같은 스케줄링 관련 정보 기억 
  * CPU Registers : PCB에는 프로세스가 실행되는 중에 사용하던 레지스터의 값이 저장.(EX) ACCUMULATOR, INDEX REGISTER, STACK POINTER). 이전에 실행할 떄 사용한 레지스터의 값을 보관해야 다음에 실행 가능-> 자신이 사용하던 레지스터의 중간값을 보관. CPU 내 범용 레지스터(AX, BX, CX, DX), 데이터 레지스터(SP, BP, SI, DI), 세그먼트 레지스터(CS, DS, ES, SS) 등이 갖고 있는 값 저장.
  * Owner(계정 정보) : 계정 번호, CPU할당 시간, CPU 사용시간 등 각종 스케줄러에 필요한 정보를 프로세스 제어블록에 저장.
  * CPU Usage : 
  * Memory Usage : 메모리 위치정보, 메모리 보호를 위해 사용하는 경계 레지스터 값, 한계 레지스터값 저장.
  
  ![process metadata](C:\Users\hw030\OneDrive\문서\GitHub\algorithm\process metadata.png)

### PCB(Process Control Block)

> **Process Metadata** 들을 저장해 놓는 곳,자료구조(TCB - Task Control Block)

​	![image-20210209073828047](C:\Users\hw030\AppData\Roaming\Typora\typora-user-images\image-20210209073828047.png) 

```
프로그램 실행 → 프로세스 생성 → 프로세스 주소 공간에 (코드, 데이터, 스택) 생성 
→ 이 프로세스의 메타데이터들이 PCB에 저장 → 프로세스 실행 완료시 폐기
```



#### PCB의 필요성

* CPU에서는 프로세스의 상태에 따라 교체작업이 이루어진다.(인터럽트가 발새해서 할당받은 프로세스가 Block 상태가 되고 다른 프로세스를 running으로 바꿀 때)



​	**이때, 앞으로 다시 수행할 Block 상태의 프로세스의 상태값을 PCB에 저장해두는 것!!**



#### PCB 관리 방식

* Linked List 방식으로 관리

* PCB List Head에 PCB들이 생성될 때마다 붙게 된다. 주소값으로 연결이 이루어져 있는 연결리스트이기 때문에 **삽입 삭제가 용이**.

  즉, 프로세스가 생성되면 해당 PCB가 생성되고 프로세스 완료시 제거된다.

* 이렇게 수행 중인 프로세스를 변경할 때, **CPU의 레지스터 정보가 변경되는 것**을 **Context Switching** 이라 한다.





### Context Switching

> CPU가 이전의 프로세스 상태를 PCB에 보관하고, 또 다른 프로세스의 정보를 PCB에 읽어 레지스터에 적재하는 과정

* 보통 인터럽트가 발생하거나, 실행 중인 CPU 사용 허가시간을 모두 소모하거나, 입출랙을 위해 대기해야 하는 경우에 Context Switching이 발생

```
즉, 프로세스가 Ready → Running, Running → Ready, Running → Waiting처럼 상태 변경 시 발생!
```



#### Context switching의 필요성

​	만약 컴퓨터가 매번 하나의 Task만 처리할 수 있다면?

​	- 다음 Task를 처리하기 위해서 현재 Task가 끝날 때까지 기다려야 한다.

​	\- 반응속도가 매우 느리고 사용하기 불편.

​	\- 컴퓨터 멀티태스킹을 통해 빠른 반응속도로 응답 가능.

​	\- 빠르게 Task를 바꾸면서 실행하기에 사람은 실시간처리가 되는 것처럼 보인다.

​	\- CPU가 Task를 바꿔가며 실행하기 위해 Context Switching이 필요하다.



#### 수행과정

1. Task의 대부분 정보는 Register에 저장되고 PCB(Process Control Block)로 관리됨.

2. 현재 실행하고 있는 Task의 PCB 정보를 저장.(Process Stack, Ready Queue)

3. 다음 실행할 Task의 PCB 정보를 읽어 Register에 적재하고 CPU가 이전에 진행했던 과정을 연속적으로 수행할 수 있게 된다.





#### Context Switching의 OverHead란?

overhead는 과부하라는 뜻으로 보통 안좋은 말로 많이 쓰인다.

하지만 프로세스 작업 중에는 OverHead를 감수해야 하는 상황이 있다.

```text
프로세스를 수행하다가 입출력 이벤트가 발생해서 대기 상태로 전환시킴
이때, CPU를 그냥 놀게 놔두는 것보다 다른 프로세스를 수행시키는 것이 효율적
```

즉, CPU에 계속 프로세스를 수행시키도록 하기 위해서 다른 프로세스를 실행시키고 Context Switching 하는 것

CPU가 놀지 않도록 만들고, 사용자에게 빠르게 일처리를 제공해주기 위한 것이다.