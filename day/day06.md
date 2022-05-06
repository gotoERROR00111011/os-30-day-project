# day 06

## 인터럽트 처리

### GDTR

```
LGDT : GDTR에 48 bit 대입
```

GDTR의 하위 16 bit는 limit (GDT의 유효 byte 수 -1)

세그먼트의 address는 32 bit

세그먼트의 limit은 세그먼트의 byte 수를 나타낸다.
limit는 최대 32 bit (4GB)이지만 limit에 주어진 bit를 초과한다.
이를 해결하기위해 G (granularity) bit flag를 붙여 byte 단위가 아닌 페이지(4KB) 단위로 해석하게한다.

세그먼트의 속성 (세그먼트 액세스 권한) 12 bit
시스템 모드, 애플리케이션 모드, 읽기, 쓰기, 실행 등의 정보를 가지고 있다.

### PIC (Programmable Interrupt Controller)

CPU 단독으로는 인터럽트를 1개 밖에 취급할 수 없는 문제를 해결하기 위한 칩이다.
8개의 인터럽트 신호(IRQ, interrupt request)를 1개의 인터럽트 신호로 정리하는 장치이다.

PIC 레지스터 IMR, ICW

PIC에서 CPU로 보내는 인터럽트는 0xcd 0x?? 2 byte로 BIOS 호출에 사용한 INT 0x?? 이다.

인터럽트 처리 종료 후에는 return;(=RET)가 아닌 IRETD 를 사용해야 한다.
buffer로는 FILO 구조를 사용한다.

```
EXTERN
CALL
```
