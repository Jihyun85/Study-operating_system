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

  2. **Associative Mattping** (연관 사상)

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
