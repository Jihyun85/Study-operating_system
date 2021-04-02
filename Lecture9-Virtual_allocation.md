## Lecture9. Virtual memory

### 가상메모리

---

### 🌞Virtual Storage (Memory)

- Non-continuous allocation
- 사용자 프로그램을 여러 개의 block으로 분할
- **실행 시, 필요한 block들만 메모리에 적재**  
  → 나머지 block들은 swap device에 존재
- 기법
  - Paging system
  - Segmentation system
  - Hybrid paging/segmentation system

### 🌞Address Mapping

: **✨Virtual address를 real address로 바꿔주는 것!**

- in Non-continuous allocation
  - **Virtual address (가상주소)** = relative address  
    → Logical address (논리주소)  
    → **연속된 메모리 할당을 가정한 주소**
  - Real address (실제주소) = absolute (physical)  
    → 실제 메모리에 적재된 주소
- 사용자/프로세스는 실행 프로그램 전체가 메모리에 연속적으로 적재되었다고 가정하고 실행 할 수 있음

### 🌞 Block Mapping

- 사용자 프로그램을 block 단위로 분할/관리  
  → 각 block에 대한 address mapping 정보 유지
- Virtual address : v = (b, d)  
  → b = block number  
  → d = displacement(offset) in a block
- Block map table (BTM)  
   → Address mapping 정보 관리 => Kernel공간에 프로세스마다 하나의 BMT를 가짐  
  <img src="https://media.vlpt.us/images/monsterkos/post/01f29a04-a5ec-41ec-ae1e-b66c523c2baa/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-09-12%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%206.44.02.png" width="550px" alt="block mapping">
  > Block Mapping의 흐름
  >
  > 1. 프로세스의 BMT에 접근
  > 2. BMT에서 block b에 대한 항목(entry)를 찾음
  > 3. Residence bit 검사  
  >    ① Residence bit = 0 경우, swap device에서 해당 블록을 메모리로 가져옴. BMT업데이트 후 3-② 단계 수행  
  >    ② Residence bit = 1 경우, BMT에서 b에 대한 real address 값 a 확인
  > 4. 실제 주소 r 계산 (r = a + b)
  > 5. r을 이용하여 메모리에 접근

### 🌞Paging System

- 프로그램을 같은 크기의 블록으로 분할 => 블록을 **Pages**라고 한다고 보면 됨!
- Terminologies
  - Page : 프로그램의 분할된 block
  - Page frame : 메모리의 분할 영역으로 Page와 같은 크기로 분할!
- 🔑특징
  - 논리적 분할이 아님 (크기에 따른 분할)
    - Page 공유(sharing) 및 보호(protetion) 과정이 복잡함(segmentation 대비)
  - Simple and Efficient(segmentation 대비)
  - No external fragmentation✨
    - Internal fragmentation 발생 가능
- Address Mapping

  - Virtual address : v = (p, d)
    - p : page number
    - d : displacement(offset)
  - Address mapping
    - PMT(Page Map Table) 사용
  - Adress mapping mechanism
    - Direct mapping (직접 사상) - block mapping과 유사
    - Associative mapping (연관 사상)  
      → TLB(Translation Look-aside Buffer)
    - Hybrid direct/associative mapping

  1. **Direct mapping** (직접 사상)

     - Block mapping 방법과 유사
     - 👹문제점  
       → 메모리 접근 횟수가 2배이므로 '성능 저하'  
       → PMT를 위한 메모리 공간 필요
     - 👩‍🎓해결방안  
       → Associative mapping (TLB)  
       → PMT를 위한 전용 기억장치(공간) 사용  
       → 기타
     - 가정  
        → PMT를 커널 안에 저장  
        → PMT entry size = entrySize  
        → Page size = pageSize
       > Direct Mapping의 흐름
       >
       > 1. 해당 프로세스의 PMT가 저장되어 있는 주소 b에 접근
       > 2. 해당 PMT에서 page p에 대한 entry 찾음  
       >    → p의 entry 위치 = b + p \* entrySize
       > 3. 찾아진 entry의 존재 비트 검사  
       >     ① Residence bit = 0인 경우(**page fault**), swap device에서 해당 page를 메모리로 적재. PMT를 갱신한 후 3-② 단계 수행  
       >    ※ page fault도 context switching 발생으로 overhead가 크다.  
       >     ② Residence bit = 1인 경우, 해당 entry에서 page frame 번호 p'를 확인
       > 4. p'와 가상 주소의 변위 d를 사용하여 실제 주소 r 형성  
       >    → r = p' \* pageSize + d
       > 5. 실제 주소 r로 주기억장치에 접근

  2. **Associative Mapping** (연관 사상)

     - TLB(Translation Look-aside Buffer)에 PMT 적재  
       → TLB: address mapping할 때 옆에 두고 쓰는 특별한 장치  
       → Associative high-speed memory
     - PMT를 병렬 탐색
     - Low overhead, high speed
     - Expensive hardware😅  
       → 큰 PMT를 다루기가 어려움

  3. **Hybrid direct/associative mapping**
     - 두 기법을 혼합하여 사용  
       → HW 비용은 줄이고, Associative mapping의 장점 활용
     - 작은 크기의 TLB 사용  
       → PMT : 메모리(커널 공간)에 저장  
       → TLB : PMT 중 일부 entry들을 적재 - ✨ 최근에 사용된 page들의 entry 저장
     - **Locality (지역성)** 활용  
        → 프로그램의 수행과정에서 한 번 접근한 영역을 다시 접근(temporal locality) 또는 인접 영역을 다시 접근(spatial locality)할 가능성이 높음
       > 프로세스의 PMT가 TLB에 적재되어 있는지 확인  
       > ① TLB에 적재되어 있는 경우  
       >  → residence bit를 검사하고 page frame번호 확인  
       > ② TLB에 적재되어 있지 않은 경우  
       >  → Direct mapping으로 page frame 번호 확인  
       >  → 해당 PMT entry를 TLB에 적재함

#### 🔑**Memory Management**

: paging system이 메모리를 어떻게 관리하는지 공부!

- Page와 같은 크기로 미리 분할하여 관리/사용
  - Page frame
  - FPM(Fixed Partition Multi-programming) 기법과 유사
- Frame table

  - Page frame당 하나의 entry
  - 구성  
    → Allocated/available field : 할당 가능? 사용가능?  
    → PID field : 실제 페이지가 올라와 있나?  
    → Link field : For free list (빈 공간을 찾음)  
    → AV : Free list header (free list의 시작점)

  * Frame table  
    → AV : 시작 pointer로 frame table에 와서 빈 공간을 찾음  
    → link : 자신 다음의 비어있는 공간에 대한 리스트가 적혀 있는 것

- Page Sharing
  - 여러 프로세스가 특정 page를 공유 가능  
    → Non-continuous allocation 이기 때문에 가능!!
  - 공유 가능 page
    1. Procedure pages : Pure code를 담고 있는 페이지
    2. Data page : read-only data / read-write data(병행 제어 기법 관리 하✨)
  - Procedure page Sharing에서 문제 발생  
    → PMT에서 동일한 위치에 존재해야 함!🤣 => 프로세스들이 shared page에 대한 정보를 PMT의 같은 entry에 저장하도록 함
  - 여러 프로세스가 page를 공유 할 때 **보안(protection)의 문제**가 생김
    - **Protection bit**로 해결!✨

#### 🔑**Paging System - Summary**

- 프로그램을 고정된 크기의 block으로 분할(page) / 메모리를 block size로 미리 분할(page frame)  
  → external pragmentation 문제 없음  
  → 메모리 통합/압축 불필요  
  → 프로그램의 논리적 구조 고려하지 않음✨  
   : Page sharing/protection이 복잡  
  → 필요한 page만 page frame에 적재하여 사용  
   : 메모리의 효율적 활용  
  → page mapping overhead  
   : 메모리 공간 및 추가적인 메모리 접근 필요  
   : 전용 HW 활용으로 해결 가능 => HW 비용 증가

### 🌞Segmentation system

- 프로그램을 **논리적 block**으로 분할 (segment)  
  (paging system은 크기에 따라 분할했었음)  
  → **Block의 크기가 서로 다를 수 있음!!!**⭐⭐⭐  
  → 예) stack,heap, main procedure, shared lib, Etc.
- 특징
  - 메모리를 미리 분할하지 않음 (VPM(Variable Partition Multi-programming)와 유사)
  - Segment sharing/protection 용이
  - Address mapping 및 메모리 관리의 overhead가 큼
  - No internal fragmentation  
    → External fragmentation 발생 가능

1. **address mapping**

   - Virtual address : v = (s, d)  
     → s : segment number  
     → d : displacement in a segment
   - Segment Map Table (SMT)
   - Address mapping mechanism  
     → Paging system과 유사

   1. direct mapping
      > Address mapping(direct mapping) 흐름
      >
      > 1. 프로세스의 SMT가 저장되어 있는 주소 b에 접근
      > 2. SMT에서 segment s의 entry 찾음  
      >    → s의 entry 위치 = b + s \* entrySize
      > 3. 찾아진 Entry에 대해 다음 단계들을 순차적으로 실행  
      >    ① 존재 비트가 0인 경우 (// missing segment fault) swap device로부터 해당 segment를 메모리로 적재, SMT를 갱신  
      >    ② 변위(d)가 segment 길이보다 큰 경우 (d > ls), segment overflow exception 처리 모듈을 호출  
      >    ③ 허가되지 않은 연산일 경우 (protection bit field 검사), segment protection exception 처리 모듈을 호출
      > 4. 실제 주소 r 계산 (r = a + d)
      > 5. r로 메모리에 접근

#### 🔑**Memory Management**

- VPM과 유사  
  → Segment 적재 시, 크기에 맞추어 분할 후 적재
- Segment sharing / protection  
  → **논리적**으로 분할되어 있어, 공유 및 보호가 용이함

#### 🔑**Segmentation System - Summary**

- 프로그램을 논리 단위로 분할 (segment) / 메모리를 동적으로 분할
  - 내부 단편화 문제 없음
  - Segment sharing / protection 용이
  - Paging system 대비 관리 overhead가 큼
- 필요한 segment만 메모리에 적재하여 사용  
  → 메모리의 효율적 활용
- Segment mapping overhead  
  → 메모리 공간 및 추가적인 메모리 접근이 필요(paging system과 마찬가지)  
  → 전용 HW 활용으로 해결 가능

### 🌞Hybrid Paging/Segmentation

- Paging과 Segmentation의 장점 결합
  - Page sharing/protection이 쉬움
  - 메모리 할당/관리 overhead가 작음
  - No external fragmentation
- 프로그램 분할  
  1)논리 단위의 segment로 분할  
  2)각 segment를 고정된 크기의 page들로 분할
- Page단위로 메모리에 적재
- 전체 테이블의 수 증가
  - 메모리의 소모가 큼
  - Address mapping 과정이 복잡
- Direct mapping의 경우, 메모리 접근이 3배  
  → 성능이 저하될 수 있음
