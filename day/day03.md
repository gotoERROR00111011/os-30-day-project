# day 03 - 32비트 모드 돌입과 C언어 도입

## Assembly

```
JC  : jump if carry
JNC : jump if not carry
JAE : jump if above or equal
JB  : jump if below
JBE : jump if below or equal
EQU : equal
RET : return
```

## Register

### Flag Register

```
FLAGS.CF : carry flag (1 bit)
```

## IPL (Initial Program Loader)

### Disk

boot sector 이외의 sector에 접근하기 위해서는 BIOS interrupt가 필요하다.

register에 목적에 맞는 값을 대입 후 disk bios 명령 `INT 0x13`으로 디스크를 읽고 memory의 buffer address에 저장한다.

다음은 disk interrupt에 사용되는 register 이다.

```
AH = 읽기, 쓰기, 찾기, 검사
AL = 처리할 sector 수
CH, CL, DH, DL : 디스크 위치 정보 (실린더, 섹터, 헤드, 드라이브)
ES:BX : buffer address
FLAGS.CF : carry flag, 실행 결과 error 검사
```

실린더, 헤드 등이 없는 다른 저장장치도 있기 때문에 별도의 interrupt로 처리할 것으로 생각된다. (혹은 호환 가능하게 register를 구성)

### Buffer Address

디스크에서 읽은 데이터를 저장할 memory address

초기에는 HW 기술적인 어려움으로 32 bit register (EBX)를 대신해 segment register를 추가로 사용했다.
`MOV AL, [ES:BX]` 처럼 사용하며 memory address는 `ES*16+BX` 으로 사용하여 범위를 확장한다.
항상 segment register와 함께 address를 지정해야 하며 생략된 경우는 대부분 `DS`가 생략되어 있다.
(`MOV CX, [1234]` = `MOV CX, [DS:1234]`)

## OS 본체

### Disk Image

1. assembly code 를 compile
1. disk image 파일로 저장
1. disk image 에서 compile 결과물 시작 위치 확인 (0x004200)

### Boot

`(disk image를 load할 memory의 시작점) + (disk image 파일 내용 시작점)` 부터 실행하면 OS 부팅이 시작된다.

책에서는 disk의 boot sector를 memory의 `0x8000`으로 지정했고, disk image의 파일 내용 시작점은 `0x4200` 이다. 따라서 `0x8000 + 0x4200 = 0xc200` 으로 jump 하여 실행한다.

### +

실제 code는 다음과 같이 실행된다.

1. IPL (boot sector)를 memory(`0x7c00 ~ 0x7dff`)에 load
1. IPL 다음인 두번째 sector 부터 memory(`0x8200 ~`)에 load
1. IPL 끝에서 `JMP 0xc200`하여 OS 본체 실행

2일째에서는 memory map에서 `0x7c00 ~ 0x7dff`이 boot sector가 읽혀지는 address라 언급하고, 3일째에서는 `0x8000`을 boot sector로 사용한다고 언급하여 혼란이 생긴다.
code 분석 결과 `0x7c00`은 IPL, `0x8200`이 OS 본체로 보인다.

## C 언어

BIOS는 16 bit 기계어로 쓰여있기 때문에 32 bit 모드에서는 사용할 수 없다.

1. .c -> .gas (cc1.exe 책의 저자가 gcc를 개조한것)
1. .gas -> .nas (gas2nask.exe .gas는 nask.exe로 번역 불가)
1. .nas -> .obj (nask.exe)
1. .obj -> .bim (obj2bim.exe)
1. .bim -> .hrb (bim2hrb.exe)
1. assembly로 만든 기계어(.bin)와 연결 (copy)

C 언어를 기계어로 만들어도 assembly로 쓴 부분과 link해야 한다.
다른 파일과 연결하기 위한 정보를 가진것이 .obj 파일 이다.
하나의 기계어로 만들기 위해서는 .obj 파일을 link 시켜야 한다.

책의 저자는 독자적으로 bim(binary image file) 파일 형식과 obj2bim.exe를 만들어 사용한다.

C언어에서 assembly를 사용하기 위해 다음과 같이 만들고, link 해주어야 한다.

```assembly
[FORMAT "WCOFF"]			; 오브젝트 파일을 만드는 모드
[BITS 32]					; 32 비트 모드용의 기계어를 만든다


; 오브젝트 파일을 위한 정보
[FILE "naskfunc.nas"]			; 원시 파일명 정보

		GLOBAL	_io_hlt			; 이 프로그램에 포함되는 함수명


; 이하는 실제의 함수
[SECTION .text]				; 오브젝트 파일에서는 이것을 쓰고 나서 프로그램을 쓴다

_io_hlt:	; void io_hlt(void);
		HLT
		RET
```
