# day 04 - C언어와 화면 표시의 연습

## Assembly

```
; void write_mem8(int addr, int data);
_write_mem8:
		MOV		ECX,[ESP+4]		; [ESP+4]에 addr가 들어가 있으므로 그것을 ECX에 read
		MOV		AL,[ESP+8]		; [ESP+8]에 data가 들어가 있으므로 그것을 AL에 read
		MOV		[ECX],AL
		RET
```

```
INSTRSET    : CPU 모드 지정 (8068, 486, ...)
```

## VRAM

메모리의 특정 영역(현재 환경에서는 0xa0000 ~ 0xaffff)은 각각의 address가 화면의 화소에 대응한다. 이 값을 변경하여 화면 표시 기능을 구현할 수 있다.

## Pointer

assembly에서 `MOV` 명령으로 대입할때 register 처럼 크기가 정해진 경우가 아니면 `BYTE, WORD, DWORD`처럼 크기를 명시해야한다.
C언어도 assembly로 변환되고, 같은 규칙을 따른다.
틀린경우, C compiler에서 error를 발생시킨다.

```assembly
;error가 발생하는 코드
MOV [addr], (addr & 0x0f)
```

```c
char  *p;  // BYTE
short *p;  // WORD
int   *p;  // DWORD
```

```c
int addr;
char *p;
// 0x0f 는 addr이 int이지만 addr 값이라는 것을 강조하기 위해 남긴것
// pointer 자체와는 무관계

// assembly
write_mem8(addr, addr & 0x0f);

// error, 크기 지정이 없다.
*addr = addr & 0x0f;

// cast 경고, memory address와 value를 별도로 취급하기 때문이다.
p = addr;
*p = addr & 0x0f;

// 정상 작동
p = (char *)addr;
*p = addr & 0x0f;

// 정상 작동, p 선언이 필요 없다.
// BYTE[addr] = addr & 0x0f
*((char *) addr) = addr & 0x0f;
```

`p[i]`는 그저 생략표현이고 본질적으로 배열과는 관계가 없다.
예를들어 다음 4가지 `p[i], *(p+i), i[p], *(i+p)`는 compile로 생선된 기계어까지 모두 같다.
(`a[2], 2[a]` 두가지 또한 같은 코드이다.)

책의 C 예제 코드에 `i`를 `1`로 잘못 기입한 경우가 여러곳 발견된다.
(부록CD 코드에서는 올바르게 입력되어 있다.)

## IN / OUT

CPU는 memory 이외에 다른 device와 연결되어 있다.
CPU 명령이기 때문에 assembly로 작성해야 한다.
정해진 device 번호(port)를 사용해 제어한다.

```
_io_in8:	; int io_in8(int port);
    MOV		EDX,[ESP+4]		; port
    MOV		EAX,0
    IN		AL,DX
    RET

_io_out8:	; void io_out8(int port, int data);
    MOV		EDX,[ESP+4]		; port
    MOV		AL,[ESP+8]		; data
    OUT		DX,AL
    RET
```

## EFLAGS

EFLAGS는 FLAGS(16 bit)를 32 bit로 확장한 것이다.
캐리 플래그, 인터럽트 플래그 등의 플래그를 포함하고 있다.
EFLAGS를 읽거나 쓰기 위해서는 PUSHFD, POPFD를 이용해야 한다.

### CLI, STI

- CLI : Clear Interrupt Flag
- STI : Set Interrupt Flag

CPU에 interrupt 요구 신호가 왔을때, interrupt 반응 회로 동작 여부(1, 0)를 설정한다.
interrupt flag를 사용한다.

<table style='text-align: center;'>
<tr>
    <td colspan='16'>FLAGS (공백 bit는 미사용)</td>
</tr>
<tr>
    <td>15</td>
    <td>14</td>
    <td>13</td>
    <td>12</td>
    <td>11</td>
    <td>10</td>
    <td>9</td>
    <td>8</td>
    <td>7</td>
    <td>6</td>
    <td>5</td>
    <td>4</td>
    <td>3</td>
    <td>2</td>
    <td>1</td>
    <td>0</td>
<tr>
<tr>
    <td></td>
    <td>NT</td>
    <td colspan='2'>IOPL</td>
    <td>OF</td>
    <td>DF</td>
    <td>IF</td>
    <td>TF</td>
    <td>SF</td>
    <td>ZF</td>
    <td></td>
    <td>AF</td>
    <td></td>
    <td>PF</td>
    <td></td>
    <td>CF</td>
<tr>

</table>

```
PUSHFD  : push flags double-word, PUSH EFLAGS
POPFD   : pop flags double-word, POP EFLAGS
PUSH
POP
RET     : 값을 return 하는 경우 RET 시점에 EAX에 들어있던 값을 return 한다.
```

## etc

컬러 지정, 사각형 그리기 등 생략
