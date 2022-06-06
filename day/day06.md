# day 06 - 분할 컴파일과 인터럽트 처리

## GDTR, IDTR

```
LGDT : GDTR에 48 bit 대입
```

GDTR은 MOV 명령으로 대입이 불가능하다.
LGDT라는 명령으로만 사용가능하다.

|   32 bit    |          16 bit           |
| :---------: | :-----------------------: |
| GDT address | limit (GDT 유효 byte - 1) |

## Segment 정보

- segment 크기, limit
  - 20 bit
- segment 시작
  - 32 bit (address)
- segemnt 속성
  - 12 bit
  - 상위 4 bit (GD00)
    - G : G비트, limit을 byte가 아닌 페이지(4KB) 단위로 해석 설정
    - D : segment mode : 16 bit, 32 bit
  - 하위 8 bit (쓰기금지, 실행금지, 시스템 전용 등)

## PIC (Programmable Interrupt Controller)

PIC는 CPU 단독으로는 인터럽트를 1개 밖에 취급할 수 없는 문제를 해결하기 위한 칩이다.
8개의 인터럽트 신호(IRQ, interrupt request)를 1개의 인터럽트 신호로 정리한다.
PIC에서 CPU로 보내는 인터럽트는 0xcd 0x?? 2 byte로 BIOS 호출에 사용한 INT 0x?? 와 동일하다.

### PIC register

- IMR : interrupt mask register
- ICW : initial control word

인터럽트 처리 종료 후에는 return;(=RET)가 아닌 IRETD 를 사용해야 한다.
buffer는 FILO 구조를 사용한다.

```
IRETD
EXTERN
CALL
```
