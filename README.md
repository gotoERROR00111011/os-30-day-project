# os-30-day-project

## Introduction

'OS 구조와 원리' 에 따라 진행하는 OS 개발 30일 프로젝트

## Register

|     |                   |
| --- | ----------------- |
| AX  | accumulator       |
| CX  | counter           |
| DX  | data              |
| BX  | base              |
| SP  | stack pointer     |
| BP  | base pointer      |
| SI  | source index      |
| DI  | destination index |
|     |                   |
| AL  | accumulator low   |
| CL  | counter low       |
| DL  | data low          |
| BL  | base low          |
| AH  | accumulator high  |
| CH  | counter high      |
| DH  | data high         |
| BH  | base high         |
|     |                   |
| ES  | extra segment     |
| CS  | code segment      |
| SS  | stack segment     |
| DS  | data segment      |
| FS  | noname            |
| GS  | noname            |

## Assembly

|      |                  |
| ---- | ---------------- |
| ;    | comments         |
| DB   | data byte        |
| DW   | data word        |
| DD   | data double word |
| RESB | reserve byte     |
| ORG  | origin           |
| JMP  | jump             |
| MOV  | move             |
| ADD  | add              |
| CMP  | compare          |
| JE   | jump if equal    |
| INT  | interrupt        |
| HLT  | halt             |
|      |                  |

## Memory

### Memory Map

| address                 | -                                  |
| ----------------------- | ---------------------------------- |
| 0x00000000 ~ 0x000fffff | 부팅 중 사용, 그 후 비어있음 (1MB) |
| 0x00100000 ~ 0x00267fff | 플로피디스크의 기억내용 (1440KB)   |
| 0x00268000 ~ 0x0026f7ff | 비어있음 (30KB)                    |
| 0x0026f800 ~ 0x0026ffff | IDT (2KB)                          |
| 0x00270000 ~ 0x0027ffff | GDT (64KB)                         |
| 0x00280000 ~ 0x002fffff | bootpack.hrb (512KB)               |
| 0x00300000 ~ 0x003fffff | 스택 등 (1MB)                      |
| 0x00400000 ~            | 비어있음                           |

## References

- OS 구조와 원리: OS 개발 30일 프로젝트 (2007-04-14) (카와이 히데미)
- 30 日でできる！ OS 自作入門 (2006/3/1) (川合 秀実)
- [부록CD](https://www.hanbit.co.kr/store/books/look.php?p_code=B9833754652#tabs_7)
- [서포트 페이지 (日)](http://hrb.osask.jp/)
