## Lecture 7. Deadlock Resolution

### 교착상태

---

### 🌞Deadlock의 개념

- Blocked/Asleep state  
  → 프로세스가 '특정 이벤트' 혹은 '필요한 자원'을 기다리는 상태
- Deadlock state  
  → 프로세스가 **발생 가능성이 없는 이벤트**를 기다리는 경우(프로세스가 deadlock 상태)  
  → 시스템 내에 deadlock에 빠진 프로세스가 있는 경우(시스템이 deadlock 상태)
- starvation과 유사  
  → starvation은 cpu를 기다리고 ready state에 있으며, 발생 가능성이 없는 것은 아니라는 점에서 차이가 있다.

### 🌞자원의 분류

- 일반적 분류 : Hardware resources vs Software resources
- 다른 분류법
  - 선점 가능 여부에 따른 분류
  - 할당 단위에 따른 분류
  - 동시 사용 가능 여부에 따른 분류
  - 재사용 가능 여부에 따른 분류

1. **선점 가능 여부**에 따른 분류

   - preemptible resources  
     → 선점 당한 후, 돌아와도 문제가 발생하지 않는 자원  
     → Processor, memory 등
   - Non-preemptible resources  
     → 선점 당하면, 이후 진행에 문제가 발생하는 자원 - Rollback, restart 등 특별한 동작이 필요  
     → E.g., disk drive 등

2. **할당 단위**에 따른 분류

   - Total allocation resources  
     → 자원 전체를 프로세스에게 할당  
     → E.g., Processor, disk drive 등
   - Partitioned allocation resources  
     → 하나의 자원을 여러 조각으로 나누어 여러 프로세스들에게 할당  
     → E.g., Memory 등

3. **동시 사용가능 여부**에 따른 분류

   - Exclusive allocation resources  
     → 한 순간에 한 프로세스만 사용 가능  
     → E.g., Processor, memory, disk drive 등  
     → 메모리의 경우 여러 조각으로 할당했지만 해당 조각 하나하나는 한 순간에 한 프로세스만 사용 가능하다.
   - Shared allocation resource  
     → 여러 프로세스가 동시에 사용 가능한 자원  
     → E.g., Program(sw), shared data 등

4. **재사용 가능 여부**에 따른 분류
   - SR (Serially-reusable Resources)  
     → 시스템 내에 항상 존재하는 자원  
     → 사용이 끝나면, 다른 프로세스가 사용 가능  
     → E.g., Processor, memory, disk drive, program 등
   - CR (Consumable Resources)  
     → 한 프로세스가 사용한 후에 사라지는 자원  
     → E.g., signal, message 등

### 🌞Deadlock과 자원의 종류

- Deadlock을 발생시킬 수 있는 자원의 형태✨
  - Non-preemptible resources
  - Exclusive allocation resources
  - Serially reusable resources  
    ※ 할당 단위는 영향을 미치지 않음!
- CR을 대상으로 하는 Deadlock model까지 생각하면 너무 복잡! => 빼고 생각

### 🌞Deadlock Model (표현법)

1. **Graph Model**

   - Node와 Edge로 구성
     - Node : 프로세스 노드(P1, P2) 자원 노드(R1, R2)
     - Edge  
       → Rj => Pi : 자원 Rj이 프로세스 Pi에 할당 됨  
       → Pi => Rj : 프로세스 Pi가 자원 Rj을 요청(대기 중)
   - cycle이 만들어지면 deadlock

2. **State Transition Model**

### 🌞Deadlock 발생 필요 조건✨

(자원의 특성)

- Exclusive use of resources
- Non-preemptible resources

(프로세스의 특성)

- Hold and wait (Partial allocation)
- Circular wait

Deadlock 해결방법 ?  
=> **Deadlock 필요 조건 중 하나를 해결하면 됨**

### 🌞Deadlock 해결 방법

- Deadlock _prevention_ methods
  → 교착상태 _예방_
- Deadlock _avoidance_ method  
  → 교착상태 _회피_
- Deadlock _detection_ and deadlock _recovery_ methods  
  → 교착상태 _탐지 및 복구_

### 🌞Deadlock prevention

- Deadlock이 절대 발생하지 않도록 하는 것
- 앞서 언급한 **deadlock 발생 필요 조건 중 하나를 제거**
- 심각한 자원 낭비 발생  
  → Low device utilization  
  → Reduced system throughput
- 비현실적✨

1. 모든 자원을 공유 허용  
   → Exclusive use of resources 조건 제거  
   → 현실적으로 불가능

2. 모든 자원에 대해 선점 허용  
   → Non-preemptible resources 조건 제거  
   → 현실적으로 불가능  
   → 유사한 방법 : 프로세스가 할당 받을 수 없는 자원을 요청한 경우, 기존에 가지고 있던 자원을 모두 반납하고 작업 취소(이후 처음(or check point)부터 다시 시작)  
   → 심각한 자원 낭비 발생 => 비현실적

3. 필요 자원 한번에 모두 할당 (Total allocation)  
   → Hold and wait 조건 제거  
   → 자원 낭비 발생 => 필요하지 않은 순간에도 갖고 있음  
   → 무한 대기 현상 발생 가능

4. Circular wait 조건 제거  
   → Totally allocation을 일반화 한 방법  
   → 자원들에게 순서를 부여  
   → 프로세스는 순서의 증가 방향으로만 자원 요청 가능✨  
   → 자원 낭비 발생

### 🌞Deadlock avoidance

→ 시스템의 상태를 계속 감시  
 → 시스템이 **deadlock 상태가 될 가능성**이 있는 자원 할당 요청 보류  
 → 시스템을 항상 **safe state**로 유지

- Safe state  
  → **모든 프로세스가 정상적 종료 가능한 상태**  
  → Safe sequence가 존재 => Deadlock상태가 되지 않을 수 있음을 존재
- Unsafe state  
  → Deadlock 상태가 될 **가능성**이 있음  
  → 반드시 발생한다는 의미는 아님!!

- 👩‍🎓가정 (Not practical함)  
  → 프로세스의 수가 고정  
  → 자원의 종류와 수가 고정됨  
  → 프로세스가 요구하는 자원 및 최대 수량을 알고 있음  
  → 프로세스는 자원을 사용 후 반드시 반납 (\_당연한 이야기)

- 방법

  1. Dijkstra's algorithm - banker's algorithm

     - Deadlock avoidance를 위한 간단한 이론적 기법
     - 가정 : 한 종류(resource type)의 자원이 여러 개(unit)
     - 시스템을 항상 safe state로 유지  
       => 자원 요청이 들어왔을 때 빌려줬다고 가정하고 safe state가 있는지 확인

  2. Habermann's algorithm
     - Dijkstra's algorithm의 확장
     - 여러 종류의 자원 고려✨
     - 시스템을 항상 safe state로 유지  
       <img src="https://media.geeksforgeeks.org/wp-content/cdn-uploads/gq/2016/01/safety.png" alt="deadlock avoidance" width="500px">

- 📒Deadlock Avoidance 정리
  - Deadlock을 막을 수 있음
  - High overhead : 항상 시스템을 감시하고 있어야 함
  - Low resource utilization : safe state 유지를 위해 사용 되지 않는 자원이 존재
  - Not practical : 프로세스 수, 자원 수가 고정. 필요한 최대 자원 수를 알고있음

### 🌞Deadlock detection

- Deadlock 방지를 위한 사전 작업을 하지 않음(Deadlock 발생 가능)
- 주기적으로 deadlock 발생 확인
  - 시스템이 deadlock 상태인가?
  - 어떤 프로세스가 deadlock 상태인가?
- Resource Allocation Graph (RAG) 사용  
  <img src="https://courses.cs.washington.edu/courses/cse451/98au/Lectures/9-deadlock/img005.JPG" width="500px" alt="RAG">
  - Graph reduction (Deadlock인지 확인하는 법)  
    : 주어진 RAG에서 edge를 하나씩 지워가는 방법
    - Completly reduced : 모든 edge 제거됨. Deadlock에 빠진 프로세스 없음.
    - Irreducible : 지울 수 없는 edge가 존재. 하나 이상의 프로세스가 deadlock 상태.
    - 특징  
      → High overhead : 검사 주기에 영향 받음 / Node의 수가 많은 경우

### 🌞Deadlock Avoidance vs Detection

- Deadlock avoidance

  - 최악의 경우를 생각 - 앞으로 일어날 일을 고려
  - Deadlock이 발생하지 않음

- Deadlock detection
  - 최선의 경우를 생각 - 현재 상태만 고려
  - Deadlock 발생 시 Recovery 과정이 필요

### 🌞Deadlock Recovery

- 두 가지 방법

  - Process termination

    - Deadlock 상태인 프로세스 중 **일부** 종료
    - Termination cost model : 종료 시킬 deadlock 상태의 프로세스 선택  
      → Termination cost

      - 우선순위 / The priority
      - 종류 / Process type
      - 총 수행 시간 / Accumulated execution time of the process
      - 남은 수행 시간 / Remaining time of the process
      - 종료 비용 / Accounting cost
      - Etc.

    - Lowest-termination cost process first  
      → simple  
      → Low overhead  
      → 불필요한 프로세스들이 종료 될 가능성이 높음

    - Minimum cost recovery  
      → 최소 비용으로 deadlock 상태를 해소 할 수 있는 process 선택  
      → 모든 경우의 수를 고려해야 하므로 complex, high overhead  
      → O(2^k)

  - Resource preemption

    - Deadlock 상태 해결을 위해 선점할 자원 선택
    - 해당 자원을 가지고 있는 프로세스를 종료 시킴  
      → Deadlock 상태가 아닌 프로세스가 종료 될 수도 있음😱😱  
      → 해당 프로세스는 이후 재시작 됨
    - 선점할 자원 선택  
      → Preemtion cost model이 필요  
      → Minimum cost recovery method 사용  
      → O(r)

  - Checkpoint-restart method
    - 프로세스의 수행 중 **특정 지점(checkpoint)마다 context를 저장**
    - Rollback을 위해 사용 : 강제 종료 후 가장 최근의 checkpoint에서 재시작
