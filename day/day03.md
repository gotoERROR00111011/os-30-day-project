# day 03

## 32 bit 모드, C 언어

### Operator

```
JC  : jump if carry
JNC : jump if not carry
JAE : jump if above or equal
JB  : jump if below
JBE : jump if below or equal

EQU : equal
```

### Register

```
register flag (1 bit)
FLAGS.CF : carry flag
```

### IPL (Initial Program Loader)

부트섹터, 즉 디스크의 첫 512 byte 다음의 섹터에 접근하기 위해서는 BIOS interrupt가 필요하다.

BIOS interrupt 는 INT(interrupt 명령)와 지정된 레지스터의 조합으로 실행한다.
예를들어 디스크 읽기의 경우 다음과 같이 레지스터 설정 후 INT 로 BIOS를 호출해 세팅된 플로피디스크(A)-실린더0-헤드0-섹터2 를 읽는다.

```
		MOV		AX,0x0820
		MOV		ES,AX
		MOV		CH, 0			; 실린더 0
		MOV		DH, 0			; 헤드 0
		MOV		CL, 2			; 섹터 2

		MOV		AH, 0x02		; 디스크 read
		MOV		AL, 1			; 1섹터
		MOV		BX,0
		MOV		DL, 0x00		; A드라이브
		INT		0x13			; 디스크 BIOS 호출
		JC		error
```

다만, 현재 32 bit, 특히 플로피디스크는 거의 사용하지 않기 때문에 BIOS interrupt 도 크게 변경되었을 것이다.
따라서 상세한 규격에 얽매이지 않는 방향으로 학습하도록 한다.

### 버퍼 어드레스

16 bit 레지스터는 메모리 접근 범위가 0~65535로 제한적이다.
이후에 32 bit 레지스터(EAX, EBX, ...)가 생기지만, 초기에는 보조역할을 하는 세그먼트 레지스터를 만들어 사용하였다.
`MOV AL, [ES:BX]` 처럼 사용하며 메모리 주소는 ES\*16+BX 으로 사용하여 범위를 확장한다.
사실 항상 세그먼트 레지스터와 함께 번지를 지정해야 하며 생략된 경우는 대부분 `DS`가 생략되어 있다.
(`MOV CX, [1234]` = `MOV CX, [DS:1234]`)
특성(DS\*16)상 DS의 값은 0으로 유지해야한다.

### OS 실행

디스크 이미지에서 OS 위치를 찾아서 로드해야한다.

우선 프로그램을 이미지 파일으로 만들고, 만든 이미지 파일을 바이너리 에디터로 열어서 프로그램 시작점을 찾는다.

부트섹터의 시작점(0x0000)에서 프로그램 시작점 사이의 거리는 메모리에 올려도 동일하다.
(디스크 전체를 순차적으로 메모리에 로드했기 때문) 메모리 위치를 계산하여 부트섹터의 과정 끝에서 프로그램 시작점 위치로 jump 하면 OS가 실행된다.
`ORG (메모리)부트섹터의 시작점 + 프로그램 시작점`  
`JMP (메모리)부트섹터의 시작점 + 프로그램 시작점`

### C 언어

C언어 코드를 기계어로 컴파일해도 단독으로는 사용하지 못하고, 어셈블리와 link 해야만 사용할 수 있다.
windows, linux 등에서 컴파일만으로 해결되는 것은, 컴파일러 내부에서 이러한 과정을 처리하기 때문이다.
C언어에 없는 기능(HLT)를 어셈블러의 규격에 맞게 작성한 코드로 link 하여 C언어에서 사용할 수도 있다.
