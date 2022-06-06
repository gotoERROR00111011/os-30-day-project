# day 00

## Index

<details>
<summary>0장 개발을 시작하기 전에</summary>

</details>
<details>
  <summary>1장 - 7장 : bootstrap, assembly, C, interrupt</summary>

- PC 구조부터 어셈블리 입문까지
- 어셈블리 학습과 Makefile 입문
- 32비트 모드 돌입과 C언어 도입
- C언어 화면와 표시의 연습
- 구조체와 문자 표시와 GDT/IDT 초기화
- 분할 컴파일과 인터럽트 처리
- FIFO와 마우스 제어
</details>
<details>
  <summary>8장 - 14장 : memory, graphic, algorithm</summary>

- 마우스 제어와 32비트 모드 전환
- 메모리 관리
- 겹치기 처리
- 마침내 윈도우
- 타이머 1
- 타이머 2
- 고해상도와 키 입력

</details>
<details>
  <summary>15장 - 21장 : multitask, console</summary>
  
  - 멀티태스크 1
  - 멀티태스크 2
  - 콘솔
  - dir 커맨드
  - 애플리케이션
  - API
  - OS 지키기

</details>
<details>
  <summary>22장 - 28장 : application</summary>

- C언어로 애플리케이션을 만들자
- 그래픽의 여러가지
- 윈도우 조작하기
- 콘솔 늘리기
- 윈도우 이동의 고속화
- LDT와 라이브러리
- 파일과 일본어 표시

</details>
<details>
  <summary>29장 - 30장 : program</summary>

- 압축과 간단한 애플리케이션
- 고도의 애플리케이션

</details>
<details>
<summary>31장 개발을 마친 후</summary>

</details>
<br>

## OS 디스크 생성 과정

1. 운영체제 소스 코드 작성
1. 기계어로 컴파일
1. 이미지 파일로 가공
1. 이미지 파일을 디스크에 작성

## HW

- IBM PC/AT 호환 기종
- 플로피디스크 (1440KB)
- QEMU 에뮬레이터를 사용한다.

## C

C compiler는 각 OS의 애플리케이션 개발을 전제로 설계되어있다.
따라서 printf 같은 OS가 제공하는 기능은 CPU가 일반보호예외를 발생시킨다.
C 언어는 다른 언어들에 비해 OS 지원 기능이 적기 때문에 OS 개발에 적합하다.

## Assembly

OS 제어용 register 제어는 assembly 로 작성해야 한다.
애플리케이션을 위한 C compiler는 OS 제어 register 제어 명령이 없기 때문이다.
