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
> - Critical section (임계 영역)  
>   → 공유 데이터를 접근하는 코드 영역
> - Mutual exclusion (상호배제)  
>   → 둘 이상의 프로세스가 동시에 critical section에 진입하는 것을 막는 것
