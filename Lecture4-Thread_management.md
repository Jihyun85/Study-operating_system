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

resource는 공유한다✨

<img src="https://2.bp.blogspot.com/-3AB4sE53Dfw/VMVNdWa_V0I/AAAAAAAAACo/UAGFO7f6_UA/s1600/euva3a00.p54z.gif" width="500px" />

메모리를 공유하고 있다는 것을 보여주는 이미지  
→ 같은 프로세스의 스레드들은 동일한 주소 공간을 공유한다.  
→ 각 stack들이 메모리 공간을 나눠 가진 것을 볼 수 있다.

#### 스레드(Thread)

- Light Weight Process (LWP)  
  → 프로세스는 프로세스인데 가볍다.  
  → 프로세스는 자원 할당 및 제어라면 스레드는 제어만 갖고 있으므로!

- 프로세서(e.g CPU) 활용의 기본 단위✨  
  → 스레드가 여러개면 동시에 여러개의 CPU를 사용할 수 있다.

- 구성요소

  - Thread ID
  - Register set (PC, SP 등)
  - Stack (i.e. local data)

- 제어 요소 외 코드, 데이터 및 자원들은 프로세스 내 다른 스레드들과 공유

- 전통적 프로세스 = 단일 스레드 프로세스✨

### 🌞스레드의 장점

- 사용자 응답성(Responsiveness)  
  : 일부 스레드 처리가 지연되어도, 다른 스레드는 작업을 계속 처리 가능  
  → ex. 게임할 때 소리, 마우스, 화면 출력 등이 동시에 일어나는 것 등

- 자원 공유 (Resource sharing)  
  : **효율성 증가** (커널 개입을 피할 수 있음)  
  → 프로세스 2개 (P1, P2)가 A라는 자원을 사용해야 한다면, P2는 P1이 사용을 끝내거나 context switch가 일어날 때까지 기다려야 함 (비용증가)  
  하지만 스레드 2개 (T1, T2)를 가진 프로세스 하나라면 T1과 T2는 A를 동시에 사용할 수 있으므로 비용이 감소한다.

- 경제성(Economy)  
  : 프로세스 생성, context switch에 비해 효율적

- 멀티 프로세서(multi-processor) 활용  
  : 병렬처리를 통해 성능 향상

### 🌞스레드의 구현

<img src="https://blog.kakaocdn.net/dn/bd0rl8/btqw3P8ORxQ/ad3YXWok3CTyOLEztCp6Fk/img.png" width="500px" />

- **사용자 수준 스레드 (User thread)**

  - 사용자 영역의 스레드 라이브러리로 구현됨  
    → 스레드의 생성, 스케줄링을 담당
    → POSIX threads, Win32 threads, Java thread API 등
    → 스레드 제어 블록(TCB)가 사용자 영역에 있음

  - 커널은 스레드의 존재를 모름

    - 💖장점 : 커널의 관리(개입)을 받지 않음  
      → 생성 및 관리 부하가 적고 유연한 관리 가능, 이식성이 높음

    - 👹단점 : 커널은 프로세스 단위로 자원 할당  
      → 하나의 스레드가 block 상태가 되면 모든 스레드가 대기해야 함 (single-threaded kernel의 경우)

- **커널 수준 스레드 (Kernel thread)**

  - OS(Kernel)가 직접 관리  
    → TCB가 커널에 있음
  - 💖장점 : 커널이 각 스레드를 개별적으로 관리  
    → 프로세스 내 스레드들이 개별적으로 수행 가능
  - 👹단점 : 커널 영역에서 스레드의 생성, 관리 수행  
    → context switching 등 부하(overhead)가 큼

- **혼합형(n:m) 스레드**
  - n개 사용자 수준 스레드 - m개의 커널 스레드 (n >= m)  
    → 사용자가 원하는 수 만큼 스레드 사용  
    → 커널 스레드는 자신에게 할당된 하나의 사용자 스레드가 block 상태가 되어도, 다른 스레드 수행 가능(UT1이 block이 되어도 UT2를 수행하면 됨)
  - 효율적이면서 유연함
