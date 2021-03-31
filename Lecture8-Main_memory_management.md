## Lecture8. Main memory management

### 메모리(주기억장치) 관리

---

### 🌞**Main memory management를 위한 background**

#### 🔸 **메모리(기억장치)의 종류**

- 레지스터 : HW가 관리(CPU)
- 캐시 : HW가 관리(CPU)
- 메인 메모리 : SW가 관리
- 보조기억장치 : SW가 관리
- (위로 올라갈 수록 속도가 빠르고 비싸다.)

#### 🔸 **메모리(기억장치) 계층구조**

: 보조기억장치에서 메인 메모리로 데이터를 올릴 때, **block** 단위로 올린다.

- Block
  - 보조기억장치와 주기억장치 사이의 데이터 전송 단위
  - size : 1 ~ 4KB
- Word
  - 주기억장치와 레지스터 사이의 데이터 전송 단위
  - size : 16 ~ 64KB
  - (32bit, 64bit 컴퓨터의 기준이라고 할 수 있음!!!)

#### 🔸 **Address Binding**

: 프로그램의 논리 주소를 실제 메모리의 물리 주소로 매핑(mapping)하는 작업

- Binding 시점에 따른 구분
  - Compile time binding
  - Load time binding
  - Run time binding

1. Compile time binding  
   → 프로세스가 메모리에 적재될 위치를 컴파일러가 알 수 있는 경우(위치가 변하지 않는다. 변할 수 없다.)  
   → 프로그램 전체가 메모리에 올라가야 함

2. Load time binding  
   → 메모리 적재 위치를 컴파일 시점에 모르면, 대체 가능한 **상대주소**를 생성  
   → 적재 시점(load time)에 시작 주소를 반영하여 사용자 코드 상의 주소를 재설정  
   → 프로그램 전체가 메모리에 올라가야 함

3. Run time binding  
   → Address binding을 수행시간까지 연기 => running상태가 될 때까지 연기(수행 도중 다른 메모리 위치로 이동할 수 있음)  
   → HW의 도움이 필요(MMU: Memory Management Unit)  
   → 대부분의 OS가 사용

#### 🔸 **Dynamic Loading**

- 모든 루틴을 교체 가능한 형태로 디스크에 저장
- 실제 호출 전까지는 루틴을 적재하지 않음  
  → 메인 프로그램만 메모리에 적재하여 수행  
  → 루틴의 호출 시점에 address binding 수행
- 장점 : 메모리 공간의 효율적 사용✨

#### 🔸 **Swapping**

- 프로세서 할당이 끝나고 수행 완료 된 프로세스는 swap-device로 보내고(Swap-out)
- 새롭게 시작하는 프로세스는 메모리에 적재 (Swap-in)

### 🌞Memory Allocation (메모리 할당)

1. Continuous Memory Allocation (연속할당)  
   → 프로세스 (context)를 하나의 연속된 메모리 공간에 할당하는 정책  
   → 메모리 구성 정책
   - 메모리에 동시에 올라갈 수 있는 프로세스 수(Multiprogramming degree)
   - 각 프로세스에게 할당되는 메모리 공간 크기
   - 메모리 분할 방법
2. Non-continuous Memory Allocation (비연속할당)

### 🌞Continuous Memory Allocation

> - Uni-programming
> - Multi-programming
>   - Fixed partition (FPM)
>   - Variable partition (VPM)

- Uni-programming  
  → 하나의 프로세스만 메모리 상에 존재  
  → 가장 간단한 메모리 관리 기법

  → 👹문제점 : 프로그램의 크기 > 메모리 크기 인 경우  
  → 👩‍🎓해결법 : Overlay structure

  - 메모리에 현재 필요한 영역만 적재(번갈아가며 적재)
  - 사용자가 프로그램의 흐름 및 자료구조를 모두 알고 있어야 함😱

  → 👹문제점 : 커널(Kernal)보호가 필요함  
  → 👩‍🎓해결법 : 경게 레지스터 (boundary register)로 막아둠

  → 👹문제점 : Low system resource utilization / Low system performance  
  → 👩‍🎓해결법 : Multi-programming

- Multi Programming

  - 🔸**Fixed Partition Multiprogramming**  
    → 메모리 공간을 고정된 크기로 분할(미리 분할되어 있음)  
    → 각 프로세스는 하나의 partition(분할)에 적재  
    → 파티션의 수(K) = 프로세스의 수 (multiprogramming degree = K)

    → 커널 및 사용자 영역 보호 : Boundary register

    → **Fragmentation (단편화)**  
     : 여기서 internal/external 구분은 partition의 내부/외부!

    - Internal fragmentation  
      : Partition 크기 > Process 크기 => 낭비되는 메모리가 생김
    - External fragmentation
      : 남은 메모리 크기 > Process 크기 인데도 **연속된 공간이 아니어서** 적재할 수 없음 => 메모리가 낭비됨

      ※ 요약  
       → 고정된 크기로 메모리 미리 분할  
       → 메모리 관리가 간편함 (low overhead)  
       → 시스템 자원이 낭비 될 수 있음 <= Internal/external fragmentation

  - 🔸**Variable Partition Multiprogramming**  
    → 초기에는 전체가 하나의 영역  
    → 프로세스를 처리하는 과정에서 메모리 공간이 **동적으로** 분할  
    → No internal fragmentation✨ (partition 내부 fragmentation이 없음!)  
    → external fragmentation은 생길 수 있음  
     : 동적으로 나눈 partition 중 남은 partition 각각의 크기 < Process 크기

    - 배치 전략 (Placement strategies)

      1. First-fit(최초 적합)  
         → 충분한 크기를 가진 **첫 번째** partition을 선택  
         → Simple and Low overhead  
         → 공간 활용률이 떨어질 수 있음 (13kb를 요구했는데 첫 번째 partition이 넓은 경우)

      2. Best-fit(최적 적합)  
         → Process가 들어갈 수 있는 partition 중 **가장 작은 곳**에서 선택  
         → 탐색시간이 오래 걸림 (모든 partition을 살펴봐야 한다.)  
          => overhead가 큼  
         → 크기가 큰 partition을 유지 할 수 있음  
         → 작은 크기의 partition이 많이 발생 (활용하기에 너무 작은 partition)

      3. Worst-fit(최악 적합)  
         → Process가 들어갈 수 있는 partition 중 **가장 큰 곳** 선택  
         → 탐색시간이 오래 걸림  
         → **작은 크기의 partition 발생을 줄일 수 있음**  
         → 큰 크기의 partition 확보가 어려움

      4. Next-fit (순차 최초 적합)  
         → 최초 적합 전략과 유사  
         → State table에서 마지막으로 탐색한 위치부터 탐색  
         → 메모리 영역의 **사용 빈도 균등화**  
         → Low overhead

    - External fragmentation을 위한 전략

      1. Coalescing holes (공간 통합)
         : **인접한 빈 영역**을 하나의 partition으로 통합  
         → process가 memory를 release하고 나가면 수행  
         → Low overhead

      2. Storage Compaction (메모리 압축)  
         : 모든 빈 공간을 하나로 통합  
         → 프로세스 처리에 필요한 적재공간 확보가 필요할 때 사용  
         → High overhead!!! : 모든 Process 재배치(Process 중지), 많은 시스템 자원을 소비
