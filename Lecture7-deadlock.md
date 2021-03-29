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
