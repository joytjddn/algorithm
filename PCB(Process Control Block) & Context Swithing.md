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

### PCB(Process Control Block)

> **Process Metadata** 들을 저장해 놓는 곳,자료구조(TCB - Task Control Block)

​	![image-20210209073828047](C:\Users\hw030\AppData\Roaming\Typora\typora-user-images\image-20210209073828047.png) 

```
프로그램 실행 → 프로세스 생성 → 프로세스 주소 공간에 (코드, 데이터, 스택) 생성 
→ 이 프로세스의 메타데이터들이 PCB에 저장
```

