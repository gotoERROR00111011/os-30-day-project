# day 05 - 구조체와 문자 표시와 GDT/IDT 초기화

## 문자 출력

문자의 모양은 bit 단위로 만든다.
출력할 위치와 매칭되는 vram에 입력하는 방식으로 출력한다.
모든 문자를 직접 만드는것은 무리가 있기 때문에 공개된 데이터를 사용한다.

## 변수

기존 OS용 debugger는 OS 개발시 debug를 못한다.
따라서 변수 값을 확인하기 위해서 sprintf를 사용한다.
이 값을 화면에 출력하며 개발 도중 문제가 없는지 확인할 수 있다.

## Segmentation

메모리를 분할한 뒤, 각 블록의 첫 address를 0으로(`ORG 0`) 하여 다루는 기능이다.

`MOV AL, [DS:EBX]`가 16 bit 에서는 (DS\*16)으로 단순히 DS의 16배 였지만, 32 bit 에서는 DS로 나타나는 segment의 시작 address이다.

segmentation이 segment 1개를 나타내기 위해서는 (segment의) 크기, 시작번지, 관리속성(쓰기금지, 실행금지, 시스템전용 등) 이 필요하다.

## GDT - global (segment) descriptor table

CPU는 segmentation 정보를 64 bit로 나타낸다.
하지만 segment register는 16 bit로 부족하다.
이 문제를 각 segment 번호에 대응하는 segment들의 정보를 memory에 담아 해결했다.
이것이 바로 GDT이다.

## IDT - interrupt descriptor table

interrupt가 발생했을때, interrupt 번호에 대응하는 기능을 호출하는 것이 IDT이다.
IDT는 segment가 설정되어야 하므로 GDT를 먼저 설정하고 IDT를 설정한다.
