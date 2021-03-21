## Lecture2 Operating System Overview

### (운영체제 개요)

---

### 🌞운영체제의 역할

- **User interface (편리성)**

  - CUI(Character User Interface) : 문자로 주고 받는 유저 인터페이스
  - GUI(Graphical User Interface) : 그림 형태로 되어 있는 유저 인터페이스(현재)
  - EUCI(End-User Confortable Interface) : 특정 목적을 위한 인터페이스\_mp3

- **Resource management (효율성)** : 주어진 자원을 잘 활용

  - HW resource (propessor, memory, I/O devices, Etc.)
  - SW resource (file, application, message, signal, Etc.)

- **Process and Thread management**
  → 프로세스(process): 실행 주체

- **System management (시스템 보호)**  
  → 사용자가 불법적인 행동 등

### 🌞컴퓨터 시스템의 구성

→ System Call Interface : 필요한 기능을 OS에 요청할 때의 통로. 커널이 제공하는 기능 중 사용자가 사용할 수 있는 기능들을 모아놓은 것들을 의미하기도 함.  
→ **kernel(커널)** : 운영체제의 핵심

### 🌞운영체제의 구분

→ (반효경 교수님의 1강 참고. 1_Operating-system.md)

#### 운영체제 발전 역사

- No OS, ~ 1940s  
  → 운영체제 개념 존재하지 않음  
  → 하드웨어 관리를 사용자가 직접했어야 함.  
  → 사용자가 기계어로 직접 프로그램 작성  
  → 실행하는 작업 별 순차 처리 됨 => 각각 작업 준비시간 소요.

- Batch Systems (1950s ~ 1960s)  
  → 일괄처리 시스템  
  → 모든 시스템을 중앙(전자계산소 등)에서 관리 운영  
  → 준비시간이 줄어듬.  
  → 시스템이 좋아함. 시스템 지향적(System-oriented)

  💖장점  
   → 많은 사용자가 시스템 자원 공유  
   → 처리 효율 향상

  👹단점  
   → 생산성 저하 - 같은 유형의 작업들이 모이기를 기다려야 함  
   → 긴 응답시간(turnaround time) - 약 6시간

- Time Sharing Systems\_시분할 시스템 (1960s ~ 1970s)  
  → 요즘 쓰는 형태도 시분할 시스템.  
  → 여러 사용자가 자원을 동시에 사용 - OS가 파일 시스템 및 가상 메모리 관리  
  → 사용자 지향적(User-oriented)  
  → 단말기(CRT terminal) 사용 - 화면을 그려주고 입력을 전달하는 역할

  💖장점  
   → 응답시간(responsive time) 단축 (약 5초\_반응)  
   → 생산성 향상 - 프로세서 유휴 시간 감소

  👹단점  
   → 통신 비용 증가 - 통선 비용, 보안 문제 등  
   → 개인 사용자 체감 속도 저하 - 동시 사용자 수 증가-시스템 부하 증가- 느려짐(개인 관점)

- Personal Computing  
  → 개인이 시스템 전체 독점  
  → CPU 활용률(utilization)이 고려의 대상이 아님  
  → OS가 상대적으로 단순함 (다양한 사용자 지원 기능이 중요해짐)

  💖장점  
   → 빠른 응답시간

  👹단점  
   → 성능(performance)이 낮음

- Parallel Processing System (병렬 처리 시스템)  
  → 단일 시스템 내에서 둘 이상의 프로세서 사용 (동시사용)  
  → 메모리 등의 자원 공유 (Tightly-coupled system)  
  → 사용 목적 : 성능 향상 / 신뢰성 향상(하나가 고장나도 정상 동작 가능)  
  → 프로세서 간 관계 및 역할 관리가 필요함

- Distributed Processing Systems (분산 처리 시스템)  
  → 네트워크(LAN)를 기반으로 구축된 병렬처리 시스템  
  → 서버 간 연결  
  → 물리적 분산  
  → 각 붙어있는 컴퓨터를 Node라고 함  
  → 분산운영체제 : Node들을 관리하는 시스템으로 하나의 프로그램, 자원처럼 사용 가능.  
  → Cluster system

  💖장점  
   → 자원 공유를 통한 높은 성능  
   → 고신뢰성, 높은 확장성

  👹단점  
   → 구축 및 관리가 어려움

- Real-time Systems (실시간 시스템)  
  → 작업 처리에 **제한 시간(deadline)** 을 갖는 시스템 (반드시 제한 시간까지 답을 줘야 함)
  → 종류 : Hard real-time task - 발전소, 무기 제어 등 / Soft real-time task

### 🌞운영체제의 구조

→ 크게 두 가지로 구분 : 커널(kernel) / 유틸리티 (Utility)

**커널(kernel)** (사전적 의미: 알맹이)  
: 👑 _OS의 핵심_ (메모리에 항상 올라가 있음)  
→ 가장 빈번하게 사용되는 기능들을 담당한다. ex) 시스템 관리(processor, memory 등)  
→ 동의어 : 핵(neucleus), 관리자(supervisor) 프로그램, 상주 프로그램 등

**유틸리티(Utility)**  
→ 메모리 비상주 프로그램  
→ UI 등 서비스 프로그램

🔑 감싸고 있는 순서 : 하드웨어 < 커널 < system calls < 유틸리티

- **단일 구조 운영체제**
  : 커널 또는 운영체제 기능을 거대한 하나의 커널로 모아놓은 것  
  (메모리 관리, 파일 시스템, 입출력, 프로세서 스케줄러 등이 하나의 커널에)

  💖 장점  
   → 커널 내 모듈간 직접 통신 (효율적 자원 관리, 사용이 가능)

  👹 단점  
   → 커널의 거대화 => 유지보수가 어려움  
   (main.js 안에 모든 기능을 때려넣은 느낌이라고 생각하면 됨)

- **계층 구조 운영체제**

  <img src="https://lh3.googleusercontent.com/proxy/LFxi-Q-Z24LaZD6xflEFbSihHGlg7vlC7lH1WbM7k44fY2OEUu7Hpn1_4pchsv1xnYPD-H3O1TPwVePVydm2O12o1hUnulOTkynMsEVInODW_XTGxvEnHCjl9QPGBI75czI9jg9mfdiGIFEtaL17uX-AX7l7b36hdlHun300XZnHzCi086MVSs-ssnp44y3eu07Q" width="360px">   
     
  💖 장점   
  → 모듈화 : 계층간 검증 및 수정 용이
  → 설계 및 구현의 단순화

  👹 단점  
  → 단일구조 대비 성능 저하 - 원하는 기능 수행을 위해 여러 계층을 거쳐야 함.

- **마이크로 커널 구조**  
  → 커널의 크기 최소화 : 필수 기능만 포함하고 기타 기능은 사용자 영역에서 수행

### 🌞운영체제의 기능 : "관리"

1. 프로세스(Process) 관리

   - 프로세스(Process)  
     → 커널에 등록된 실행 단위 (실행 중인 프로그램)  
     → 사용자 요청/ 프로그램의 수행 **주체** (entity)

   - OS의 프로세스 관리 기능  
     → 생성/삭제, 상태관리  
     → 자원 할당  
     → 프로세스 간 통신 및 동기화(synchronization)  
     → 교착상태(deadlock) 해결 : 여러개의 프로세스가 하나의 자원을 가지고 싸우는 상태를 교착상태라고 함.

   - 프로세스 정보 관리  
     → PCB

2. 프로세서(Processor) 관리

   - 프로세서는 CPU라고 생각해도 무방함
   - 프로세스 스케줄링

3. 메모리(Memory) 관리

   - 주기억장치(memory) : 작업을 위한 프로그램 및 데이터를 올려놓는 공간

   - Multi-user, Multi-tasking 시스템
     → 프로세스에 대한 메모리 할당 및 회수  
     → 메모리 여유 공간 관리  
     → 각 프로세스의 할당 메모리 영역 접근 보호

   - 메모리 할당 방법(schema)  
     → 전체 적재 - 장점: 구현이 간단함 / 단점: 공간이 제한적  
     → 일부 적재(virtual memory concept) - 장점: 메모리 효율적 사용 / 단점: 보조기억 장치 접근 필요

4. 파일(file) 관리

   - 파일 : 논리적 데이터 저장 단위
   - 사용자 및 시스템의 파일 관리
   - 디렉토리 구조 지원
   - 파일 관리 기능

5. 입출력 장치(I/O devices) 관리

   - 입출력 과정은 반드시 OS를 거쳐야 한다.

6. 기타
   - Disk
   - Networking
   - Security and Protection system 등
