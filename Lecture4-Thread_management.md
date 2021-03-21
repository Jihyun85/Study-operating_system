## Lecture4 Thread Management

### 스레드 관리

---

thread의 사전적 의미 : 실

### 🌞프로세스(Process) vs 스레드(Thread)

프로세스는 자원(resource)을 할당 받고, 그 자원을 제어해서 원하는 목적을 달성함.

스레드는 그 중 제어만 떼어놓은 것을 말한다. ('~' 과 같은 그림으로 표현)  
→ **한 개의 프로세스 안에 여러 개의 스레드가 있을 수 있다.**

### 🌞스레드(Thread)

<img src="https://blog.kakaocdn.net/dn/eQVNCM/btqLsbWenFy/q98CQGmPKUnUFLbBmP2CAk/img.png" width="500px" />

프로세스 중 제어정보(SP, PC(Program counter), 상태 등)

지역변수를 사용하는 이유 : 제어를 위해!  
이와 같이 프로세스 안에서 지역 데이터를 이용함
