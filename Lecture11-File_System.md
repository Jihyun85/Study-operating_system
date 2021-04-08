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
  - Sub0directory 생성 불가능 \_ 여전히 file naming 문제가 남음
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
