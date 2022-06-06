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

| address                 | -   |
| ----------------------- | --- |
| 0x00000000 ~ 0x00000000 | ?   |
| 0x00007c00 ~ 0x00007dff | IPL |
| 0x00007dff ~ 0x00008000 |     |
| 0x00008000 ~            | OS  |

## References

- OS 구조와 원리: OS 개발 30일 프로젝트 (2007-04-14) (카와이 히데미)
- 30 日でできる！ OS 自作入門 (2006/3/1) (川合 秀実)
- [부록CD](https://www.hanbit.co.kr/store/books/look.php?p_code=B9833754652#tabs_7)
- [서포트 페이지 (日)](http://hrb.osask.jp/)
