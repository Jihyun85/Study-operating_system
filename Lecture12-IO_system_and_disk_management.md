## Lecture12 - I/O system and Disk Management

### 입출력 시스템 & 디스크 관리

---

> - I/O mechanisms  
>   → How to send data between processor and I/O device
> - I/O Services of OS  
>   → OS Supports for better I/O performance
> - Disk Scheduling  
>   → Improve throughput of a disk
> - RAID Architecture  
>   → Improve the performance and reliability of disk system

### 🌞I/O System (HW) \_입출력시스템

- 메인메모리 ↔ 프로세서 ↔ 입출력제어기  
  ⇒ 메인메모리에서 읽어와 출력하거나 입력을 받아 메인메모리에 전달할 때 모두 프로세서를 거친다.

### 🌞I/O Mechanism

> 1.  Processor controlled memory access
>     - Polling
>     - Interrupt
> 2.  Direct Memory Access(DMA)

1. 🔑 **Processor(CPU) controlled memory access**

   - **Polling 기법** (Programmed I/O)  
     : Processor가 주기적으로 I/O 장치의 상태 확인

     - 모든 I/O 장치를 순환하면 확인
     - 전송 준비 및 전송 상태 등  
       → 👑장점 : simple, I/O 장치가 빠르고, 데이터 전송이 잦은 경우 효율적  
       → 👹단점 : Processor의 부담이 큼 (polling overhead) ⇒ 계속 순환해야 함

   - **Interrupt**  
     : I/O장치가 작업을 완료한 후, 자신의 상태를 Processor에게 전달 ⇒ Interrupt 발생시 Processor는 데이터 전송 수행  
     → 👑장점 : Pooling 대비 low overhead, 불규칙적인 요청 처리에 적합  
     → 👹단점 : Interrupt handling overhead

   - Processor controlled memory access의 아쉬운 점  
     : Processor가 모든 데이터 전송을 처리해야 하므로 overhead가 클 수 있음

2. 🔑 **Direct Memory Access (DMA)**  
   : I/O 장치와 Memory 사이의 데이터 전송을 _Processor 개입 없이_ 수행
   - Processor는 데이터 전송의 시작/종료만 관여  
     <image src="https://blog.kakaocdn.net/dn/Wpcc5/btqATg1R4oJ/aGWtqlkX6k0M0DeUIIjeq1/img.png" alt="DMA" width="500px">

### 🌞I/O Services of OS \_OS가 어떤 서포트를 하는가?

(✨✨커널 입출력 서브시스템✨✨)

1. I/O Scheduling

   - 입출력 요청에 대한 처리 순서 결정  
     → 시스템 전반적 성능 향상  
     → Process의 요구에 대한 공평한 처리  
     → ex) Disk I/O scheduling

2. Error handling

   - 입출력 중 발생하는 오류 처리  
     → ex) disk access fail, network communication error 등

3. I/O device information managements

4. Buffering⭐

   - I/O 장치와 Program 사이에 전송되는 데이터를 **Buffer에 임시 저장**
   - 전송 속도 (or 처리 단위) 차이 문제 해결

5. Caching

   - 자주 사용하는 데이터를 미리 복사해 둠(예측)
   - Cache hit시 I/O를 생략 할 수 있음

6. Spooling
   - 한 I/O 장치에 여러 program이 요청을 보낼 시, 출력이 섞이지 않도록 하는 기법  
     → 각 Program에 대응하는 disk file에 기록 (spooling)  
     → Spooling이 완료 되면, spool을 한번에 하나씩 I/O 장치로 전송  

