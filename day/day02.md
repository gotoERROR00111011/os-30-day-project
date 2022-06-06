# day 02 - 어셈블러 학습과 Makefile 입문

## Assembler

memory map에서 부트섹터가 읽혀지는 address를 ORG로 지정한다.

assembly 에서 label은 ORG 명령으로 계산된 memory의 address이다.

```
ORG : origin 메모리 로드 시작점 지정
JMP : jump
MOV : move 대입
ADD : add
CMP : compare
JE  : jump if equal
INT : interrupt
HLT : halt
```

## Register

register는 CPU 내부의 기억회로이다.

```
16 bit
AX  : accumulator
CX  : counter
DX  : data
BX  : base
SP  : stack pointer
BP  : base pointer
SI  : source index
DI  : destination index

8 bit : 16 bit 레지스터의 0-7bit(low), 8-15bit(high)
AL  : accumulator low
CL  : counter low
DL  : data low
BL  : base low
AH  : accumulator high
CH  : counter high
DH  : data high
BH  : base high
```

### Segment Register 16 bit

```
ES  : extra segment
CS  : code segment
SS  : stack segment
DS  : data segment
FS  : noname
GS  : noname
```

| 0000 | 0001 | 0010 | 0011 | 0100 | 0101 | 0110 | 0111 |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| EAX  | EAX  | EAX  | EAX  | EAX  | EAX  | EAX  | EAX  |
|      |      |      |      | AX   | AX   | AX   | AX   |
|      |      |      |      |      |      | AL   | AL   |
|      |      |      |      | AH   | AH   |      |      |

## Memory

### Memory Address

memory map을 참고하여 0번지(BIOS가 사용하는 메모리영역)처럼 사용하면 안되는 영역에 접근하지 않도록 해야한다.

`데이터의 크기 [address]` 형태로 해당 크기의 memory address의 값을 참조할 수 있다.
address 값을 가지는 register BX, BP, SI, DI를 대신 사용할수도 있다.

memory에 저장될 때에는 아래 byte 부터 저장된다.

`MOV WORD[100], 123` (123 : 00000000 01111011)

| memory   | address |
| -------- | ------- |
| ...      | ...     |
| ...      | 99      |
| 01111011 | 100     |
| 00000000 | 101     |
| ...      | ...     |

## BIOS (Basic Input Output System)

PC 기판 상에 있는 ROM에 들어있다.
BIOS 함수를 실행시키기 위해서는 각 함수에서 지정된 register와 interrupt 값을 사용한다.
지정된 register에 값을 대입, 지정된 번호로 interrupt를 실행하면 동작한다.

## make, Makefile

GNU 에서 만들었다.
assembly, c 코드 -> binary -> img 와 같이 여러차례 사용하는 명령을 관리하는데 편리한 도구이다.

```
target … : prerequisites …
        recipe
```

## Code

```assembly
; hello-os
; TAB=4

	ORG		0x7c00			; 이 프로그램이 어디에 Read되는가

; 이하는 표준적인 FAT12 포맷 플로피 디스크를 위한 기술

	JMP		entry
	DB		0x90

--- (중략) ---

; 프로그램 본체

entry:
	MOV		AX, 0			; 레지스터 초기화
	MOV		SS,AX
	MOV		SP,0x7c00
	MOV		DS,AX
	MOV		ES,AX

	MOV		SI,msg
putloop:
	MOV		AL,[SI]
	ADD		SI, 1			; SI에 1을 더한다
	CMP		AL,0
	JE		fin
	MOV		AH, 0x0e		; 한 글자 표시 function
	MOV		BX, 15			; 칼라 코드
	INT		0x10			; 비디오 BIOS 호출
	JMP		putloop
fin:
	HLT						; 무엇인가 있을 때까지 CPU를 정지시킨다
	JMP		fin				; Endless Loop

msg:
	DB		0x0a, 0x0a		; 개행을 2개
	DB		"hello, world"
	DB		0x0a			; 개행
	DB		0
```
