## Lecture5 Process Scheduling   
### 프로세스 스케줄링 -- CPU 할당
---

### 🌞다중프로그래밍 (Multi-programming)
**왜 프로세스 스케줄링을 해야하는가?**
- 여러개의 프로세스가 시스템 내 존재
- 자원을 할당할 프로세스를 "선택" 해야 함 => 스케줄링(Scheduling)
- 자원 관리
  + 시간 분할(time sharing) 관리   
  → 하나의 자원을 여러 스레드들이 번갈아 가며 사용 ex.프로세서   
  → 프로세스 스케줄링 : 프로세서 사용시간을 프로세스들에게 분배
  + 공간 분할(space sharing) 관리   
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

CPU burst > I/O burst  =>  computed-bounded   
CPU burst < I/O burst  =>  I/O-bounded   
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
