# day 01

## Assembler

memory map을 참고하여 0번지(BIOS가 사용하는 메모리영역)처럼 사용하면 안되는 영역에 접근하지 않도록 해야한다.

### Register

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

Segment Register 16 bit
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

### Operator

```
ORG : origin 메모리 로드 시작점 지정
JMP : jump
MOV : move 대입
CMP : compare
JE  : jump if equal
INT : interrupt
HLT : halt
```

### Memory Address

메모리를 지정할 경우 '데이터의크기 [address]' 형태로 사용한다.  
(MOV는 같은 bit 수에만 대입 가능한 규칙 떄문에 데이터의크기 는 생략 가능하다.)

(데이터의크기 : BYTE, WORD, DWORD)  
(address : 100, 120, ..., SI, BX, ...)

`MOV WORD[100], 123` (123 : 0000000001111011) 이 메모리에 저장될 때에는 아래 bit의 01111011이 저장되고 다음으로 00000000이 저장된다.

| memory   | address |
| -------- | ------- |
| ...      | ...     |
| ...      | 99      |
| 01111011 | 100     |
| 00000000 | 101     |
| ...      | ...     |

### Label

label 은 address 이다.

```
JMP fin
...
fin:
```

##

```asm
; hello-os
; TAB=4

		ORG		0x7c00			; 이 프로그램이 어디에 Read되는가

; 이하는 표준적인 FAT12 포맷 플로피 디스크를 위한 기술

		JMP		entry
		DB		0x90
		DB		"HELLOIPL"		; boot sector의 이름을 자유롭게 써도 좋다(8바이트)
		DW		512			; 1섹터 크기(512로 해야 함)
		DB		1			; 클러스터 크기(1섹터로 해야 함)
		DW		1			; FAT가 어디에서 시작될까(보통은 1 섹터째부터)
		DB		2			; FAT 개수(2로 해야 함)
		DW		224			; 루트 디렉토리 영역의 크기(보통은 224엔트리로 한다)
		DW		2880			; 드라이브 크기(2880섹터로 해야 함)
		DB		0xf0			; 미디어 타입(0xf0로 해야 함)
		DW		9			; FAT영역 길이(9섹터로 해야 함)
		DW		18			; 1트럭에 몇개의 섹터가 있을까(18로 해야 함)
		DW		2			; 헤드의 수(2로 하지 않으면 갈 수 없다)
		DD		0			; 파티션을 사용하지 않기 때문에 여기는 반드시 0
		DD		2880			; 드라이브 크기를 한번 더 write
		DB		0,0,0x29		; 잘 모르지만 이 값으로 해 두면 좋은 것 같다
		DD		0xffffffff		; 아마, 볼륨 시리얼 번호
		DB		"HELLO-OS   "		; 디스크 이름(11바이트)
		DB		"FAT12   "		; 포맷 이름(8바이트)
		RESB	18				; 우선 18바이트를 비어 둔다

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
		HLT					; 무엇인가 있을 때까지 CPU를 정지시킨다
		JMP		fin			; Endless Loop

msg:
		DB		0x0a, 0x0a		; 개행을 2개
		DB		"hello, world"
		DB		0x0a			; 개행
		DB		0

		RESB	0x7dfe-$			; 0x7dfe까지를 0x00로 채우는 명령

		DB		0x55, 0xaa
```
