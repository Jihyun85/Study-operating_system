## Lecture12 - I/O system and Disk Management

### 입출력 시스템 & 디스크 관리

---

> - I/O mechanisms  
>   → How to send data between processor and I/O device
> - I/O Services of OS  
>   → OS Supports for better I/O performance
> - Disk Scheduling  
>   → Improve throughput of a disk
> - RAID Architecture  
>   → Improve the performance and reliability of disk system

### 🌞I/O System (HW) \_입출력시스템

- 메인메모리 ↔ 프로세서 ↔ 입출력제어기  
  ⇒ 메인메모리에서 읽어와 출력하거나 입력을 받아 메인메모리에 전달할 때 모두 프로세서를 거친다.

### 🌞I/O Mechanism

> 1.  Processor controlled memory access
>     - Polling
>     - Interrupt
> 2.  Direct Memory Access(DMA)

1. 🔑 **Processor(CPU) controlled memory access**

   - **Polling 기법** (Programmed I/O)  
     : Processor가 주기적으로 I/O 장치의 상태 확인

     - 모든 I/O 장치를 순환하면 확인
     - 전송 준비 및 전송 상태 등  
       → 👑장점 : simple, I/O 장치가 빠르고, 데이터 전송이 잦은 경우 효율적  
       → 👹단점 : Processor의 부담이 큼 (polling overhead) ⇒ 계속 순환해야 함

   - **Interrupt**  
     : I/O장치가 작업을 완료한 후, 자신의 상태를 Processor에게 전달 ⇒ Interrupt 발생시 Processor는 데이터 전송 수행  
     → 👑장점 : Pooling 대비 low overhead, 불규칙적인 요청 처리에 적합  
     → 👹단점 : Interrupt handling overhead

   - Processor controlled memory access의 아쉬운 점  
     : Processor가 모든 데이터 전송을 처리해야 하므로 overhead가 클 수 있음

2. 🔑 **Direct Memory Access (DMA)**  
   : I/O 장치와 Memory 사이의 데이터 전송을 _Processor 개입 없이_ 수행
   - Processor는 데이터 전송의 시작/종료만 관여  
     <image src="https://blog.kakaocdn.net/dn/Wpcc5/btqATg1R4oJ/aGWtqlkX6k0M0DeUIIjeq1/img.png" alt="DMA" width="500px">

### 🌞I/O Services of OS \_OS가 어떤 서포트를 하는가?

(✨✨커널 입출력 서브시스템✨✨)

1. I/O Scheduling

   - 입출력 요청에 대한 처리 순서 결정  
     → 시스템 전반적 성능 향상  
     → Process의 요구에 대한 공평한 처리  
     → ex) Disk I/O scheduling

2. Error handling

   - 입출력 중 발생하는 오류 처리  
     → ex) disk access fail, network communication error 등

3. I/O device information managements

4. Buffering⭐

   - I/O 장치와 Program 사이에 전송되는 데이터를 **Buffer에 임시 저장**
   - 전송 속도 (or 처리 단위) 차이 문제 해결

5. Caching

   - 자주 사용하는 데이터를 미리 복사해 둠(예측)
   - Cache hit시 I/O를 생략 할 수 있음

6. Spooling
   - 한 I/O 장치에 여러 program이 요청을 보낼 시, 출력이 섞이지 않도록 하는 기법  
     → 각 Program에 대응하는 disk file에 기록 (spooling)  
     → Spooling이 완료 되면, spool을 한번에 하나씩 I/O 장치로 전송

### 🌞 Disk Scheduling

- Disk access 요청들의 *처리 순서*를 결정
- 목적: Disk system의 성능을 향상

  - 평가기준
    - Throughput : 단위 시간당 처리량
    - Mean response time : 평균 응답 시간
    - Predictability : 응답 시간의 예측성 / 요청이 무기한 연기(starvation)되지 않도록 방지

- Data access time : Seek time + Rotational delay + Data transmission time  
  → data transimission time은 항상 같으므로 seek time과 rotational delay를 향상시키도록 해야함⭐

#### 🔑**Disk Scheduling 방법**

> - Optimizing seek time
>   - FCFS (First Come First Service)
>   - SSTF (Shortest Seek Time First)
>   - Scan
>   - C-Scan (Circular Scan)
>   - Look
> - Optimizing rotational delay
>   - Sector queueing (SLTF, Shortest Latency Time First)
> - SPTF (Shortest Positioning Time First)

### 🌞 Optimizing Seek time

1. First Come First Servies (FCFS)  
   : 요청이 도착한 순서에 따라 처리  
    → 👑장점 : Simple, 공평한 처리 기법(무한 대기 방지)  
    → 👹단점 : 최적 성능 달성에 대한 고려가 없음
   → Disk access 부하가 적은 경우에는 적합!

2. Shortest Seek Time First (SSTF)  
   : 현재 head 위치에서 가장 가까운 요청 먼저 처리  
    → 👑장점 + Throughput ↑  
    + 평균 응답 시간 ↓  
    → 👹단점  
    + Predictability ↑ (언제 처리될 지 알 수 없음)  
    + Starvation 현상 발생 가능  
    → 일괄처리 시스템에 적합!

3. Scan Scheduling

   - 현재 head의 **진행 방향**에서, head와 가장 가까운 요청 먼저 처리
   - (진행방향 기준) 마지막 cylinder(끝을 말함) 도착 후, 반대 방향으로 진행

   → 👑장점

   - SSTF의 starvation 문제 해결
   - Throughput 및 평균 응답시간 우수  
     → 👹단점
   - 진행 방향 반대쪽 끝의 요청들의 응답시간 ↑

4. C-Scan Scheduling

   - Scan과 유사
   - Head가 미리 정해진 방향으로만 이동 (마지막 cylinder 도착 후 시작 cylinder로 이동 후 재시작)

   → 👑장점 : Scan대비 균등한 기회 제공

5. Look Scheduling

   - Elevator algorithm
   - Scan(C-Scan)에서 현재 진행 방향에 요청이 없으면 방향 전환
     - 마지막 cylinder까지 이동하지 않음
     - Scan (C-Scan)의 실제 구현 방법

   → 👑장점 : Scan의 불필요한 head 이동 제거

### 🌞 Optimizing Rotational Delay

1. Shortest Latency Time First (SLTF)

   - Fixed head disk 시스템에 사용  
     → 각 track마다 head를 가진 disk✨  
     → Head의 이동이 없음(필요없음)
   - Sector queuing algorithm  
     → 각 sector(track sector임 그 sector 아님) 별 queue 유지  
     → Head 아래 도착한 sector의 queue에 있는 요청을 먼저 처리 함
   - Moving head disk의 경우에도 사용 가능
   - 같은 cylinder 또는 track에 여러 개의 요청 처리를 위해 사용 가능

2. Shortest Positioning Time First (SPTF)

- Positioning time = Seek time + rotational delay
- Positioning time이 가장 작은 요청 먼저 처리

→ 👑장점: Throughput ↑, 평균 응답 시간 ↓  
 → 👹단점: 가장 안쪽과 바깥쪽 cylinder의 요청에 대해 starvation 현상 발생가능

- Eschenbach scheduling  
  : starvation 문제를 해결하기 위해 나온 알고리즘
