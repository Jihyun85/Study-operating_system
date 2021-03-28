## Lecture6 - Process Synchronization and Mutual Exclusion

### 프로세스 동기화 & 상호배제

---

### 🌞 Process Synchronization (동기화)

- 다중 프로그래밍 시스템  
  → 여러 개의 프로세스들이 존재  
  → 프로세스들은 서로 독립적으로 동작  
  → 공유 자원 또는 데이터가 있을 때, 문제 발생 가능

- 동기화 (Synchronization) => 프로세스끼리 대화하는 행위  
  → 프로세스들이 서로 동작을 맞추는 것  
  → 프로세스들이 서로 정보를 공유하는 것

### 🌞 Asynchronous and Concurrent P's

- 비동기적(Asynchronous)  
  : 프로세스들이 서로에 대해 모름 (독립적으로 동작하므로 모름)
- 병행적(Concurrent)  
  : 여러 개의 프로세스들이 동시에 시스템에 존재

※ 정리  
 : **병행 수행중인 비동기적 프로세스들이 공유자원에 동시접근 할 때 문제가 발생 할 수 있음!!😱**

### 🔑 용어정리

> - Shared data or Critical data (공유 데이터)  
>   → 여러 프로세스들이 공유하는 데이터
> - Critical section (CS, 임계 영역)  
>   → 공유 데이터를 접근하는 코드 영역
> - Mutual exclusion (상호배제)  
>   → 둘 이상의 프로세스가 동시에 critical section에 진입하는 것을 막는 것

### 🌞 Critical Section

기계어 명령(machine instruction)의 특성  
 : Atomicity(원자성), Indivisible(분리불가능)  
 : **한 기계어 명령의 실행 도중에 인터럽트 받지 않음**

### 🌞 Mutual Exclusion Methods

- Mutual exclusion primitives  
  (primitives : '기본연산' 이라고 해석하면 됨)
  - **enterCS()** primitive  
    → Critical section 진입 전 검사  
    → 다른 프로세스가 critical section 안에 있는지 검사
  - **exitCS()** primitive  
    → Critical section을 벗어날 때의 후처리 과정  
    → Critical section을 벗어남을 시스템이 알림

### 🌞 Requirements for ME(상호배제) primitives

- Mutual exclusion (상호배제)  
  → Critical Section(CS)에 프로세스가 있으면, 다른 프로세스의 진입을 금지
- Progress (진행)  
  → CS 안에 있는 프로세스 외에는 다른 프로세스가 CS에 진입하는 것을 방해하면 안됨
- Bounded waiting (한정대기)  
  → 프로세스의 CS진입은 유한시간 내에 허용되어야 함

### 🌞 Mutual Exclusion Solutions

🔸 **SW solutions**

1. Dekker's algorithm (Peterson's algorithm)  
   : Process가 두 개일 때 ME를 보장하는 최초의 알고리즘
2. Peterson's Algorithm  
   : Dekker의 알고리즘보다 간단하게 구현함
3. N-Process Mutual Exclusion
   - 다익스트라(Dijkstra)  
     → 최초로 프로세스 n개의 상호배제 문제를 소프트웨어적으로 해결  
     → 실행시간이 가장 짧은 프로세스에 프로세서 할당하는 세마포 방법, 가장 짧은 평균 대기시간 제공

- 👹SW solution들의 문제점
  - 속도가 느림
  - 구현이 복잡함
  - ME primitive 실행 중 preemption될 수 있음  
    → 공유 데이터 수정 중 interrupt를 억제하여 해결 가능 (overhead 발생)
  - **Busy waiting** - 굉장히 비효율적

🔸 **HW Solution**
SW만으로는 비효율적이어서 나옴

- TestAndSet (TAS) instruction  
  → Test와 Set을 한번에 수행하는 기계어  
  → Machine instruction => 실행 중 interrupt를 받지 않음(preemption되지 않음)  
  → Busy waiting - 비효율적

- 💖HW solution의 장점 : 구현이 간단하다
- 👹HW solution의 단점 : **Busy waiting**  
  → Busy waiting 문제를 해결하기 위한 상호배제 기법 : **Semaphore**(대부분의 OS들이 사용하는 기법)

🔸 **OS supported SW solution**

1.  Spinlock

    - 정수 변수
    - 초기화, P(), V()연산 으로만 접근 가능  
       → 위 연산들은 indivisible (or atomic) 연산  
       → OS support  
       → 전체가 한 instruction cycle에 수행 됨

      > 👩‍🎓 연산  
      > S : 물건의 개수(가져갈 자원이 있는가?)  
      > P : 물건을 꺼내가는 것(자물쇠를 거는 것)  
      > V : 물건을 집어넣는 것(자물쇠를 푸는 것)

          P(S) {
            while (S<=0) do
            endwhile;
            S <- S-1;
          }

          V(S) {
            S <- S + 1;
          }

    - 👹문제점  
      → 한 번 들어간 프로세스를 멈추지 않으므로(대기마저도) 대기가 길어지면 들어간 프로세스, 기다리는 프로세스 모두 일을 못 함  
      => 따라서 '멀티 프로세서 시스템'에서만 사용 가능
      → Busy waiting!

2.  👑**Semaphore**👑

    - 1965년 _다익스트라(Dijkstra)_ 가 제안
    - Busy waiting 문제 해결!

    - 음이 아닌 정수형 변수(S)

      - 초기화 연산, P(), V()로만 접근 가능
      - P : Probern (검사)
      - V : Verhogen (증가)

    - **임의의 S 변수 하나에 ready queue 하나가 할당 됨**

    - 🔑두 가지 구분

      - Binary Semaphore  
        → S가 0과 1 두 종류의 값만 갖는 경우  
        → 상호배제나 프로세스 동기화의 목적으로 사용

      - Counting semaphore  
        → S가 0이상의 정수값을 가질 수 있는 경우  
        → Producer-Consumer 문제 등을 해결하기 위해 사용

    > 👩‍🎓 연산
    >
    > - 초기화 연산 : S 변수에 초기값을 부여하는 연산
    > - P() 연산, V() 연산 -- 아래 코드블록 확인
    > - 모두 indivisible 연산
    >   - OS support
    >   - 전체가 한 instruction cycle에 수행 됨

        // P(S)연산
        if (S > 0)
          then S <- S - 1;
        else wait on the queue Qs; // spinlock에서는 돌지만 ready queue에서 대기함

        // V(S)연산
        if (어떤 wating processes on Qs)
          then wakeup one of them;
        else S <- S + 1;

    - Semaphore로 해결 가능한 동기화 문제들

      - 상호배제 문제 (Mutual exclusion)

      - 프로세스 동기화 문제 (process synchronization problem)  
        → process들의 _실행 순서_ 맞추기

      - ✨생산자-소비자 문제 (producer-consumer problem)  
        → 생산자 프로세스 : 메세지를 생성  
        → 소비자 프로세스 : 메세지를 전달받는 프로세스 그룹

      - ✨Reader-writer 문제  
        → Reader : 데이터에 대해 읽기 연산만 수행 (여러명 상관X)  
        → Writer : 데이터에 대해 갱신 연산을 수행 (여러명이면 큰일)  
        → 데이터 무결성 보장 필요 => Writer 상호배제 필요

      - Dining philosopher problem

      - 기타

    - 📕특징
      - **No busy waiting**  
        : 기다려야 하는 프로세스는 block(asleep)상태가 됨
      - Semaphore queue에 대한 wake-up 순서는 비결정적  
        : Starvation problem이 있을 수 있다.

3.  Eventcount/Sequencer  
    : 은행의 번호표와 비슷한 개념

- Sequencer  
  : 번호 뽑는 기계로 생각하면 됨  
  → 정수형 변수  
  → 생성시 0으로 초기화, 감소하지 않음  
  → 발생 사건들의 순서 유지  
  → ticket() 연산으로만 접근 가능

- ticket(S)\_S는 Sequence 번호  
  → 현재까지 ticket() 연산이 호출 된 횟수를 반환  
  → Indivisible operation

- Eventcount  
  : 은행에서 번호부르는 기계를 생각하면 됨  
  → 정수형 변수  
  → 생성시 0으로 초기화, 감소하지 않음  
  → 특정 사건의 발생 횟수를 기록  
  → read(E), advance(E), await(E, v) 연산으로만 접근 가능

  - read(E) : 현재 Eventcount값 반환
  - advance(E) : E <- E + 1, E를 기다리고 있는 프로세스를 깨움(wake-up)
  - await(E, v)  
    → V는 정수형 변수  
    → if (E < v) 이면 E에 연결된 Qe에 프로세스 전달(push) 및 CPU scheduler 호출

  * 📕특징
    - No busy waiting
    - No starvation  
      → FIFO scheduling for Qe
    - Semaphore 보다 더 low-level control이 가능

**여기까지 모두 Low-level mechanisms** - flexible, Difficult to use(에러 확률)

High level mechnisms => **Language-level solution**

### 🌞 Language-Level solution

그 중 하나 'Monitor'에 대해서만 강의
