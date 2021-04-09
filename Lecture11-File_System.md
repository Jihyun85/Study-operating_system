## Lecture11. File System

---

> ### Outline
>
> - Disk System
> - File System
> - Directory Structure
> - File Protection
> - Allocation Methods
> - Free Space Management

---

### 🌞 Disk System

#### 🔑**Disk pack**

- 데이터 영구 저장 장치 (비휘발성)
- 구성

  - Sector : 데이터 저장/판독의 물리적 단위
  - Track : Platter 한 면에서 중심으로 같은 거리에 있는 sector들의 집합
  - Cylinder : 같은 반지름을 갖는 track의 집합
  - Platter  
    → 양면에 자성 물질을 입힌 원형 금속판  
    → 데이터의 기록/판독이 가능한 기록 매체
  - Surface : Platter의 윗면과 아랫면

  <img src="https://blog.kakaocdn.net/dn/50daA/btqPYTxKDF8/3xXx6DqlwfeyUDQ93JMKCk/img.png" width="450px">

#### 🔑**Disk drive**

- Disk pack에 데이터를 기록하거나 판독할 수 있도록 구성된 장치
- 구성

  - Head : 디스크 표면에 데이터를 기록/판독
  - Arm : Head를 고정/지탱
  - Positioner (boom) : Arm을 지탱, Head를 원하는 track으로 이동
  - Spidle : Disk pack을 고정 (회전축), 분당 회전 수 (RPM, Revolutions Per Minute)

  <img src="https://user-images.githubusercontent.com/37871541/78574512-0d656400-7865-11ea-9b48-ccb8e23b1d65.png" width="450px">

#### 🔑**Disk Address**

1. Physical disk address  
   → Sector (물리적 데이터 전송 단위)를 지정  
   → Cylinder Number | Surface Number | Sector Number 이런 식으로 물리적 주소 지정

2. Logical disk address: relative address  
   → Disk system의 데이터 전체를 *block들의 나열*로 취급  
    ( B₁ | B₂ | B₃ | B₄ | <- 이런 식으로)
   - Block에 번호 부여
   - 임의의 block에 접근 가능  
     → Block 번호 ⇒ physical address 모듈 필요 (disk driver)

- Disk Address Mapping  
  : OS 입장에서 Disk는 Block들의 집합이므로, Disk에 접근할 때 Block 번호를 전달 ⇒ Disk driver에서 Physical address로 변환해서 Disk controller에 접근

#### 🔑**Data Access in Disk System**

1. Seek time  
   : 디스크 head를 필요한 cylinder로 이동하는 시간
2. Rotational delay  
   : 1번 이후부터, 필요한 sector가 필요한 head 위치로 도착하는 시간
3. Data transmission time  
   : 2번 이후부터, 해당 sector를 읽어서 전송(or 기록)하는 시간

- 1~3번 시간을 모두 더한 것을 data access time이라고 한다!✨✨

### 🌞File System

: 사용자들이 사용하는 파일을 관리하는 운영체제의 한 부분

- 파일시스템을 구성하는 요소
  - Files : 연관된 정보의 집합
  - Directory structure : 시스템 내 파일들의 정보를 구성 및 제공
  - Partitions : Directory들의 집합을 논리적/물리적으로 구분

### 🌞 1. **File** Concept

- **보조 기억 장치에 저장된 연관된 정보들의 집합**
  - 보조 기억 장치 할당의 _최소 단위_
  - Sequence of bytes (물리적 정의)
- 내용에 따른 분류
  - Program file : source program / object program, executable files
  - Data file
- 형태에 따른 분류
  - Text (ascii) file
  - Binary file (0과 1로 구성된 파일)
- File attributes (속성)  
  : name, identifier, type, location, size, protection 등
- File operation  
  : Create, Write, Read, Reposition, Delete, Etc.
- ✨✨ **OS**는 file operation들에 대한 **system call**을 제공해야 함

#### 🔑**File Access Methods**

- Sequential access (순차 접근)  
  : File을 record(or bytes) 단위로 순서대로 접근
- Directed access (직접 접근)  
  : 원하는 block을 직접 접근
- Indexed access  
  : Index를 참조하여 원하는 block을 찾은 후 데이터에 접근

#### 🔑**Directory**

- File들을 *분류, 보관*하기 위한 개념
- Operations on directory  
  : Search, Create, Delete, List a Directroy, Rename, Traverse the file system

#### 🔑**Partitions**(minidisk, volumes)

- Virtual disk

#### 🔑(용어) Mounting

: 현재 FS에 다른 FS를 붙이는 것  
 (안드로이드에서 SD카드를 넣고 뺄 때 마운팅 이야기가 나오기도 함)

### 🌞Directory Structure

#### 🔑**Flat Directory Structure**

- FS 내에 하나의 directory만 존재(최초의 mp3느낌)
- Issues

  - File naming
  - File protection
  - File management

  ※ 다중 사용자 환경에서 문제가 더욱 커짐

#### 🔑**2-Level Directory Structure**

- 사용자마다 하나의 directory 배정
- 구조
  - MFD (Master File Directory)
  - UFD (User File Directory)
- Problems
  - Sub-directory 생성 불가능 \_ 여전히 file naming 문제가 남음
  - 사용자간 파일 공유 불가

#### 🔑**Hierarchical Directory Structure**

- Tree 형태의 계층적 directory 사용 가능
- 사용자가 하부 directory 생성/관리 기능
  - System call(OS)이 제공되어야 함
  - Terminologies
    - Home directory(root), Current directory
    - Absolute pathname(절대경로: home부터 목표까지 적는 것), Relative pathname(상대경로: current directory기준 )
- 대부분의 OS가 사용⭐

#### 🔑**Acyclic Graph Directory Structure**(루프가 만들어지지는 않음)

- hierachical directory structure 확장
- Directory안에 shard directory, shared file를 담을 수 있음
- **Link**의 개념 사용(약간 windows의 바로가기 느낌)

#### 🔑**General Graph Directory Structure)**(루프 가능)

- Acyclic Graph Directory Structure의 일반화
- 👹무한 loop 발생할 수 있음

### 🌞File Protection

: File에 대한 부적절한 접근 방지 ⇒ 다중 사용자 시스템에서 더욱 필요!

- 접근 제어가 필요한 연산들

  - Read (R)
  - Write (W)
  - Execute (X)
  - Append (A)

- 🔑**File Protection Mechanism**  
  ① Password 기법

  - 각 file들에 PW부여
  - 비현실적  
    → 사용자들이 모든 비밀번호를 기억하기 어려움  
    → 접근 권한 별로 서로 다른 PW를 부여 해야 함
  - 중요한 파일에는 사용하고 있음

  ② Access Matrix  
   : 범위(domain)와 개체(object)사이의 접근 권한을 명시  
   → domain: 사용자 / object: file 이라고 생각하면 쉬움

  - Terminologies

    - Object : 접근 대상 (file, device 등 HW/SW objects)
    - Domain (protection domain)  
      → 접근 권한의 집합, 같은 권한을 가지는 그룹(사용자, 프로세스)
    - Access right(접근 권한)  
      → \<object-name, right-set>

    <image src="https://www.researchgate.net/profile/Md-Kamrul-Hasan-4/publication/50346030/figure/tbl3/AS:667678335320072@1536198328582/Access-control-matrix.png" alt="access right" width="500px">

  - Implementation(구현 방법)

    - Global Table  
      : 시스템 전체 file들에 대한 권한을 Table로 유지  
      ⇒ table 통째로 저장하기 때문에 빈공간까지 같이 저장하게 됨. 즉 size가 엄청 커짐👹👹
    - Access list (빈공간 저장x) - column 단위 저장  
      : Access matrix의 열(column)을 list로 표현  
      → 각 object에 대한 접근 권한 나열 (ex. <D1, R1> <D2, R2>)  
      → 권한이 없으면 적을 이유가 없음
      - Object 생성 시, 각 domain에 대한 권한 부여
      - Object 접근 시 권한을 검사✨
      - 실제 OS에서 많이 사용됨 (UNIX 터미널에서 ls -l 해보면 앞쪽에서 확인 가능)
      - 아쉬운점😅: 파일에 access 할 때마다 체크해야 하는 overhead 발생
    - Capability list (빈공간 저장x) - row 단위 저장  
      : Access matrix의 행(row)을 list로 저장  
      → 각 domain에 대한 접근 권한 나열 (ex. <F1, R1>, <F2, R2>) ⇒ 신분증 같은 느낌
      - Capability를 가짐이 권한을 가짐을 의미  
        → 프로세스가 권한을 제시, 시스템이 검증 승인
      - 👹단점: 시스템이 capability list 자체를 보호해야 함(kernel안에 저장) ⇒ overhead 발생
    - Lock-key Mechanism  
      : Access list + Capability list 개념

      - Object는 Lock을, Domain은 Key를 가짐
      - Domain 내 프로세스가 object에 접근 시, Domain의 key와 object의 lock 짝이 맞아야 함
      - 👹시스템은 key list를 관리해야 함(여전히 overhead 존재)

    - 🔸 비교

      1. Global table : simple, but can be large
      2. Access list : Object 별 권한 관리 용이 / 모든 접근 마다 권한 검사 필요(많이 접근하면 느려짐)
      3. Capability list : List 내 object들에 대한 접근에 유리 / 👹Object 별 권한 관리(권한 취소 등)가 어려움

    - 🔸 결론  
      : 많은 OS가 Access list와 Capability list 개념을 함께 사용  
       → Object에 대한 첫 접근 - access list 탐색 - 접근 허용 시 Capability 생성 후 해당 프로세스에 전달(이후 접근 시에는 권한 검사 불필요)- 마지막 접근 후 Capability 삭제

### 🌞Allocation Methods

> - Continuous allocation
> - Discontinuous allocation

1. Continuous Allocation  
   : 한 파일을 디스크의 연속된 block에 저장

- 👑장점: 효율적인 file 접근 (순차, 직접 접근)
- 👹문제점
  - 새로운 file을 위한 공간 확보가 어려움
  - External fragmentation
  - File 공간 크기 결정이 어려움  
    → 파일이 커져야 하는 경우 고려해야 함

2. Discontinuous allocation

- 1. Linked Allocation

  - File이 저장된 block들을 linked list로 연결 → 비연속 할당 가능
  - Directory는 각 file에 대한 첫 번째 block에 대한 포인터를 가짐
  - Simple, No external fragmentation
  - 👹단점
    - 직접 접근에 비효율적
    - 포인터(또는 링크) 저장을 위한 공간 필요
    - 신뢰성 문제 → 사용자가 포인터를 실수로 건드리는 문제 등
  - 많이 쓰임 ⇒ **FAT**
    - File Allocation TAble (FAT) : 각 block의 시작 부분에 다음 블록의 번호를 기록하는 방법
    - MS-DOSm Windows 등에 사용 됨

- 2. Indexed Allocation (목차가 있는 것!)
  - File이 저장된 block의 정보(pointer)를 Index block에 모아 둠
  - 직접 접근에 효율적 (순차 접근에는 비효율적)
  - file 당 Index block을 유지  
    → Space overhead  
    → Index block의 크기에 따라 파일의 최대 크기가 제한 됨
  - Unix 등에 사용 됨

### 🌞Free Space Management(빈 공간 찾기!)

1. Bit Vector  
   : 시스템 내 모든 block들에 대한 사용 여부를 1 bit flag로 표시(비트맵)

   - Simple and efficient
   - Bit vector 전체를 메모리에 보관 해야 함 ⇒ 대형 시스템에 부적합(overhead)
     <image src="https://www.researchgate.net/profile/Zbigniew-Zielinski-2/publication/266615094/figure/fig3/AS:392083035705346@1470491288218/Bit-vector-transformation-to-a-set-of-arcs.png" width="500px" alt="bit vector">

2. Linked list  
   : 빈 block을 linked list로 연결 ⇒ 공간, 시간 면에서 모두 비효율적👹

3. Grouping  
   : n개의 빈 block을 그룹으로 묶고, 그룹단위로 linked list로 연결

   - 연속된 빈 block을 쉽게 찾을 수 있음

4. Counting  
   : 연속된 빈 block들 중 첫 번째 block의 주소와 연속된 block의 수를 table로 유지

- _Continuous allocation 시스템에 유리한 기법_
