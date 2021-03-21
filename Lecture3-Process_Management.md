## Lecture3 Process Management

### 프로세스 관리

---

윈도우즈 작업관리자를 열면 **프로세스** 라고 써있는 탭이 있다.

### 🌞Job vs Process

- 작업(Job) / 프로그램  
  → 실행할 프로그램 + 데이터  
  → 아직 disk에 있음

- 프로세스  
  → 실행을 위해 시스템(커널)에 등록된 작업(Job, Program)  
  → 시스템 성능 향상을 위해 커널에 의해 관리 됨.

### 🌞프로세스의 정의\_ 여러가지로 정의할 수 있음

- **실행중인 프로그램**
  - 커널에 등록되고 커널의 관리하에 있는 작업
  - 각종 자원들을 요청하고 할당 받을 수 있는 개체
  - 프로세스 관리 블록(PCB)을 할당 받은 개체
  - 능동적인 개체(active entity) : 실행 중에 각종 자원을 요구, 할당, 반납하며 진행

### 🌞프로세스의 종류

- 역할에 따른 구분 : 시스템(커널)프로세스 / 사용자 프로세스
- 병행 수행 방법에 따른 구분 : 독립 프로세스 / 협력 프로세스

### 🌞자원(resource)의 개념

: 커널의 관리 하에 프로세스에게 할당/반납 되는 수동적 개체(passive entity)  
→ 자원의 분류 : H/W resource, S/W source(signal, message, files Etc.)

### 🌞Process Control Block (PCB)

: OS가 프로세스를 컨트롤 하기 위해 필요한 정보들을 모아놓은 공간  
→ 커널이 관리하는 영역에 있음  
→ 프로세스가 하나 생성됨 => PCB가 하나 생성됨

- PCB가 관리하는 정보  
  → PID : Process Identification Number. 프로세스 고유 식별 번호  
  → 스케줄링 정보 : 프로세스 우선순위 등과 같은 정보들  
  → 프로세스 상태 : 자원 할당, 요청 정보 등  
  → 메모리 관리 정보 : page table, segment table 등  
  → 입출력 관리 정보 : 할당 받은 입출력 장치, 파일 등에 대한 정보 등  
  → 문맥 저장 영역 (context save area) : 프로세스의 레지스터 상태 저장 공간 등  
  → 계정 정보 : 자원 사용 시간 등을 관리 (다중 사용자 시스템의 경우)

※ PCB 정보는 OS별로 서로 다름
※ PCB 참조 및 갱신 속도는 OS의 성능을 결정하는 중요한 요소이다.

### 🌞프로세스의 상태 (Process States)

: 자원 간의 상호작용에 의해 결정

1. **Created State** (생성된 상태)  
   → 사용 가능한 메모리 공간에 따라 'Ready'로 갈 지, "Suspended ready"로 갈 지 결정

   - 작업(Job)을 커널에 등록
   - PCB 할당 및 프로세스 생성
   - 커널 : **가용 메모리 공간** 체크 및 프로세스 상태 전이

2. **Ready State**  
   → 프로세서(CPU) 외에 다른 모든 자원을 할당 받은 상태  
   → 즉시 실행 가능 상태  
   → CPU를 할당 받으면 'Running' 상태로 올라감 (dispatch, schedule)

3. **Running State**
   → 프로세서(CPU)와 필요한 자원을 모두 할당 받은 상태

   - Preemption : running에서 ready로 쫓겨남(CPU)
   - Block/sleep : running에서 'asleep'으로 감\_I/O 등 자원 할당 요청(I/O 기다리는 상태)

4. **Blocked/Asleep State**  
   → 프로세서(CPU) 외에 다른 자원을 기다리는 상태  
   → 필요한 자원이 와도 바로 running 상태로 가지 않음!!!👹 - ready 상태로 가서 다시 running 상태로 올라가길 기다림 (asleep -> ready로 가는 것을 wake-up이라고 함)

5. **Suspended State**  
   → Memory image를 swap device에 보관  
   (다음에 memory에 다시 올릴 때 처음부터 하지 않기 위해 memory의 상태를 이미지로 보관해 둠)  
   → suspended 상태가 되는 것 - swap-out(suspend)  
   → suspended 상태에서 벗어나는 것 - swap-in(resume) - **suspended ready** : created에서 메모리를 할당 받지 못한 상태 - **suspended blocked** : asleep에서 메모리를 빼앗긴 상태

6. **Terminated/Zombie State**  
   → 프로세스 수행이 끝난 상태  
   → **모든 자원 반납 후**, 커널 내에 일부 PCB정보만 남아 있는 상태 (이후 프로세스 관리를 위해 보관)  
   → zombie state라고도 하는 이유, 프로세스가 끝났는데 termanted 상태로 잠시 살아있어서!  
   → running에서 terminated 상태가 되는 것 - exit

### 🌞인터럽트 (Interrupt)

→ **Unexpected, external events (예상치 못한, 외부에서 발생한 이벤트)**  
(→ 방해받음)

- 인터럽트의 종류

  - I/O interrupt : 예상하지 못한 순간에 마우스나 키보드 클릭 등
  - Clock interrupt : CPU 등의 clock
  - Console interrupt : 콘솔창
  - Program check intterrupt
  - Machine check interrupt
  - Inter-process interrupt
  - System call Interrupt

- 인터럽트 처리 과정  
  인터럽트 발생  
  ↓  
  (커널 개입)  
  프로세스 중단 및 context saving(진행 상황 저장)  
  ↓  
  인터럽트 처리(interrupt handling)  
  ↓  
  interrupt handling  
  (인터럽트 발생 장소, 원인 파악 → 인터럽트 처리/무시 여부 결정)  
  ↓  
  interrupt service  
  (인터럽트 서비스 루틴(interrupt service routine)호출)  
  ↓  
  ready 상태에 있던 프로세스 중 하나가 들어옴(기존 프로세스가 아닐 수 있다.)

### 🌞Context Switching (문맥 교환, 흐름 저장)

- Context : 프로세스와 관련된 정보들의 집합

  - CPU register context => in CPU
  - Code & data, Stack, PCB => in memory

- Context saving : 현재 프로세스의 Register context를 저장하는 작업  
  → 인터럽트에 의해 CPU에서 일하던 것을 뺏김 - memory의 PCB에 저장

- Context restoring  
  → Register context를 프로세스로 복구하는 작업

- Context switching  
  → 실행 중인 프로세스의 context를 저장하고, 앞으로 실행할 프로세스의 context를 복구 하는 일 (커널의 개입으로 이루어짐)

### 🌞Context Switch Overhead

- Context switching에 소요되는 비용  
  → OS마다 다름  
  → OS 성능에 큰 영향을 줌

- 불필요한 Context switching을 줄이는 것이 중요  
  → **스레드(thread) 사용** 등
