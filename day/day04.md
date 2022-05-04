# day 04

## C언어와 화면 표시

### VRAM

메모리의 특정 영역(현재 환경에서는 0xa0000 ~ 0xaffff)은 각각의 address가 화면의 화소에 대응한다. 이 값을 변경하여 화면 표시 기능을 구현할 수 있다.

### 포인터

어셈블리와 C의 포인터의 대응이 잘 표현되어 있지만, C 예제 코드에 `i`를 `1`로 잘못 기입한 경우가 여러곳 발견된다.
(부록CD 코드에서는 올바르게 입력되어 있다.)

### EFLAGS

EFLAGS는 FLAGS(16 bit)를 32 bit로 확장한 것이다.
캐리 플래그, 인터럽트 플래그 등의 플래그를 포함하고 있는데, 인터럽트 플래그는 캐리 플래그의 JC, JNC와 달리 EFLAGS를 읽어들여 해당 비트(9번째)가 0 or 1 인지 체크해야한다.
EFLAGS에 접근시 PUSHFD, POPFD를 통해야 한다.

```
PUSHFD  : push flags double-word
POPFD   : pop flags double-word
PUSH
POP
```
