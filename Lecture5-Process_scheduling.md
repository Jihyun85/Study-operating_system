## Lecture5 Process Scheduling

### 프로세스 스케줄링 -- CPU 할당

---

### 🌞다중프로그래밍 (Multi-programming)

**왜 프로세스 스케줄링을 해야하는가?**

- 여러개의 프로세스가 시스템 내 존재
- 자원을 할당할 프로세스를 "선택" 해야 함 => 스케줄링(Scheduling)
- 자원 관리
  - 시간 분할(time sharing) 관리  
    → 하나의 자원을 여러 스레드들이 번갈아 가며 사용 ex.프로세서  
    → 프로세스 스케줄링 : 프로세서 사용시간을 프로세스들에게 분배
  - 공간 분할(space sharing) 관리  
    → 하나의 자원을 분할하여 동시에 사용 ex. 메모리

### 🌞스케줄링(Scheduling)의 목적

✨✨✨**시스템의 성능 향상**✨✨✨

성능이라는 개념은 굉장히 모호하므로 기준이 필요함!

#### 대표적 시스템 성능 지표(index)

1. 응답시간(response time)  
   : 작업 요청으로부터 응답을 받을 때까지의 시간  
   → ex. 사용자 대화형 시스템, realtime 시스템 등에서 중요.

2. 작업 처리량(throughput)  
   : 단위 시간 동안 완료된 작업의 수  
   → ex. batch 시스템

3. 자원 활용도(resource utilization)  
   : 주어진 시간동안 자원이 활용된 시간  
   → ex. 비싼 장비를 사용할 때 등

**무엇이 가장 중요한가요?**  
→ "목적"에 맞는 지표를 고려하여 스케줄링 기법을 선택해야 한다. ( = 목적에 따라 더 중요한 지표가 달라진다.)

이외에도 성능 지표는 훨씬 많음.

### 🌞대기시간, 응답시간, 반환시간 - 용어 정리

<img src="https://t1.daumcdn.net/cfile/tistory/995760465EE9E71834" width=500px>

### 🌞스케줄링 기준 (Criteria)

: 스케줄링 기법이 고려하는 항목들

- 프로세스(process)의 특성  
  → I/O-bounded or compute-bounded ❓❓❓(아래에서 설명)
- 시스템 특성  
  → Batch system or interactive system
- 프로세스의 긴급성(urgency)  
  → Hard- or soft- real time, non-real time systems
- 프로세스의 우선순위 (priority)
- 프로세스 총 실행 시간 (total service time)
- ... 등

### 🌞CPU burst vs I/O burst

프로세스 수행 = CPU 사용 + I/O 대기(사용)  
(프로세스는 연산 → 입출력 → 연산 → 입출력을 반복하며 수행됨)

- CPU burst : CPU 사용 시간
- I/O burst : I/O 대기 시간

CPU burst > I/O burst => computed-bounded  
CPU burst < I/O burst => I/O-bounded  
→ 중요한 스케줄링 기준 중 하나!!

### 🌞스케줄링의 단계(level)

: 발생하는 빈도 및 할당 자원에 따른 구분

- Long-term scheduling  
  → 장기 스케줄링  
  → Job Scheduling

- Mid-term scheduling  
  → 중기 스케줄링  
  → Memory allocation

- Short-term scheduling  
  → 단기 스케줄링  
  → Process scheduling

### 🌞Long-term Scheduling

- Job scheduling : 시스템에 제출 할(kernel에 등록 할) 작업(Job) 결정  
  → Admission scheduling, High-level scheduling

- 다중프로그래밍 정도(degree) 조절  
  → 시스템 내에 프로세스 수 조절👏 => 성능 결정 중요 요소 중 하나!

- I/O-bounded와 computed-bounded 프로세스들을 잘 섞어서 선택해야 함  
  → 한 가지만 많이 올려놓으면 I/O나 CPU가 놀게 될 수 있음!  
  → 시스템의 효율에 중요

- 시분할 시스템에서는 모든 작업을 시스템에 등록하므로 Long-term scheduling이 상대적으로 덜 중요하다.

### 🌞Mid-term Scheduling

- 메모리 할당 결정(Memory allocation)  
  → suspended ready에서 memory를 할당해주면 ready 상태가 됨(swap-in) => **누구에게 줄 것인가⁉**

### 🌞Short-term Scheduling

- Process scheduling  
  → Low-level scheduling  
  → 프로세서를 할당할 프로세스를 결정(ready 상태에서 running으로 갈 때)

- 가장 빈번하게 발생✨  
  → Interrupt, block(I/O), time-out, Etc.  
  → 매우 빨라야 한다. (이 과정이 느리면 전체적으로 느려짐)

### 🌞스케줄링 정책(Policy)

1. preemptive(선점) / Non-preemptive(비선점) Scheduling

   - Non-preemptive scheduling : 빼앗을 수 없음  
     → 할당 받은 자원을 스스로 반납할 때까지 사용  
     → 💖장점 : Context switch overhead가 적음  
     → 👹단점 : 잦은 우선순위 역전, 평균 응답 시간 증가

   - Preemptive scheduling : 빼앗을 수 있음!  
     → 타의에 의해 자원을 빼앗길 수 있음. ex\_ 할당 시간 종료, 우선순위가 높은 프로세스 등장  
     → Context switch overhead가 크다  
     → Time-sharing system, real-time system에 적합 => 응답성 높음

2. Priority(우선순위)

   - Static priority(정적 우선순위)  
     → 프로세스 생성 시 결정된 우선순위가 유지 됨  
     → 💖장점 : 구현이 쉽고 overhead가 적음  
     → 👹단점 : 시스템 환경 변화에 대한 대응이 어려움

   - Dynamic priority (동적 우선순위)  
     → 프로세스의 상태 변화에 따라 우선순위 변경  
     → 💖장점 : 시스템 환경 변화에 유연한 대처 가능  
     → 👹단점 : 구현이 복잡, 우선순위 재계산 overhead가 큼

### 🌞기본 스케줄링 알고리즘들

1. **FCFS (First-Come-First-Service)** - 선착순 알고리즘

   - Non-preemptive scheduling

   - 스케줄링 기준 (Criteria)  
     → 도착 시간 (ready queue 기준)  
     → 먼저 도착한 프로세스를 먼저 처리
   - 자원을 효율적으로 사용 가능 (High resource utilization)  
     → scheduling overhead가 낮음, CPU가 계속 일 하게 됨
   - Batch system에 적합, interactive system에 부적합
   - 👹단점
     - **Convoy effect**  
       : 하나의 수행시간이 긴 프로세스에 의해 다른 프로세스들이 긴 대기시간을 갖게 되는 현상 (대기시간 >> 실행시간)
     - 긴 평균 응답시간(response time)
   - NTT(Nomalizaed Turnaround Time) : burst time 대비 기다린 시간에 대해 나타내주는 지표(1초가 가장 이상적)  
     <img src="https://www.includehelp.com/operating-systems/images/FCFS-2.jpg" width=500px>

2. **RR (Round-Robin)**

   - Preemptive scheduling
   - 돌아가면서 쓰자!

   - 스케줄링 기준(Criteria) --- 여기까지는 FCFS와 비슷해보임  
     → 도착 시간 (ready queue 기준)  
     → 먼저 도착한 프로세스를 먼저 처리
   - **자원 사용 제한 시간(time quantum)이 있음**
     - System parameter (δ)
     - 프로세스는 할당된 시간이 지나면 자원 반납 (Time-runout)
     - Context switch overhead가 큼
   - 대화형, 시분할 시스템에 적합
   - Time quantum (δ)이 시스템 성능을 결정하는 핵심 요소
     - Very large (infinite) δ => FCFS와 같음
     - Very small time quantum => processor sharing과 같음  
       → 체감 프로세서 속도 = 실제 프로세서 성능의 1/n  
       → High context switch overhead
   - time quantum이 끝나면 time queue에서 "맨 뒤에" 줄을 서야 함  
     <img src="https://ecomputernotes.com/images/Round-robin-Result-Table.png" width=500px>

3. SPN (Shortest-Process-Next)

   - Non-preemptive scheduling

   - 스케줄링 기준(Criteria)
     → 실행시간 (burst time 기준)
     → burst time이 가장 작은 프로세스를 먼저 처리
   - 💖장점
     - 평균 대기시간(WT) 최소화
     - 시스템 내 프로세스 수 최소화  
       → 스케줄링 부하 감소, 메모리 절약 => 시스템 효율 향상!
     - 많은 프로세스들에게 빠른 응답 시간 제공
   - 👹단점
     - Starvation (무한대기) 현상 발생  
       → BT(burst time)이 긴 프로세스는 자원을 할당 받지 못 할 수 있음
     - 정확한 실행시간을 알 수 없음 => 실행시간 예측 기법이 별도 필요

4. SRTN (Shortest Remaining Time Next)

   - SPN의 변형
   - Preemptive scheduling  
     → 잔여 실행 시간이 더 적은 프로세스가 ready 되면 선점됨
   - 💖장점
     - SPN의 장점 극대화
   - 👹단점
     - 프로세스 생성시, 총 실행 시간 예측이 필요함  
       → 잔여 실행을 계속 추적해야 함 => "overhead"
     - Context switching overhead
   - _구현 및 사용이 비현실적_

5. HRRN (High-Response-Ratio-next)

   - SPN의 변형  
     → SPN + **Aging concept**, Non-preemptive scheduling
   - Aging concepts  
     → 프로세스의 대기 시간(WT)을 고려하여 기회를 제공  
     → age ... 나이(대기 시간) 고려
   - 스케줄링 기준 (Criteria)  
     → **Response ratio**가 높은 프로세스 우선
   - **Response ratio = (WT + BT) / BT** <- 응답률 이라고 함  
     → SPN의 장점 + Starvation 방지  
     → 실행 시간 예측 기법 필요 (overhead)

6. MLQ (Multi-level Queue)
   - 작업 (or 우선순위)별 별도의 ready queue를 가짐(**여러 개 ready queue**)  
     → 최초 배정 된 queue를 벗어나지 못함  
     → 각각의 queue는 자신만의 스케줄링 기법 사용
   - Queue 사이에는 **우선순위** 기반의 스케줄링 사용
   - 💖장점  
     → 중요한 프로세스는 빨리 처리해줄 수 있다
   - 👹단점  
     → 여러 개의 Queue 관리 등 스케줄링 overhead  
     → 우선순위가 낮은 queue는 starvation 현상 발생 가능
7. MFQ (Multi-level Feedback Queue)
   - 프로세스의 Queue간 이동이 허용된 MLQ
   - Feedback을 통한 우선 순위 조정  
     → 현재까지의 프로세서 사용 정보(패턴) 활용
   - 특성
     - Dynamic priority
     - Preemptive scheduling
     - Favor short burst-time processes
     - Favor I/O bounded processes
     - Improve adaptability
   - 프로세스에 대한 사전 정보 없이 SPN, SRTN, HRRN 기법의 효과를 볼 수 있음
   - 👹단점  
     → Starvation문제  
     → 설계 및 구현이 복잡, 스케줄링 overhead가 큼
   - 변형
     - 각 준비 큐마다 시간 할당량을 다르게 배정  
       → 프로세스의 특성에 맞는 형태로 시스템 운영 가능
     - 입출력 위주 프로세스(I/O-bounded)들을 상위 단계의 큐로 이동, 우선 순위 높임 => CPU를 잠깐만 쓰고 나오므로 효율적  
       → 프로세스가 block될 때 상위의 준비 큐로 진입하게 함  
       → 시스템 전체의 평균 응답 시간 줄임, 입출력 작업 분산 시킴
     - 대기 시간이 지정된 시간을 초과한 프로세스들을 상위 큐로 이동  
       → 에이징 (aging) 기법
