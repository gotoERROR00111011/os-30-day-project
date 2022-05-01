# os-30-day-project

## Introduction

'OS 구조와 원리' 에 따라 진행하는 OS 개발 30일 프로젝트

## References

- OS 구조와 원리: OS 개발 30일 프로젝트 (2007-04-14) (카와이 히데미)
- 30 日でできる！ OS 自作入門 (2006/3/1) (川合 秀実)
- [부록CD](https://www.hanbit.co.kr/store/books/look.php?p_code=B9833754652#tabs_7)
- [서포트 페이지 (日)](http://hrb.osask.jp/)

## DAY

### DAY 00

- 책의 발매일과 현재의 개발환경(HW, CPU, 32bit-64bit)의 호환성 문제로 에뮬레이터를 사용한다.
- C컴파일러는 각 OS의 애플리케이션 개발을 전제로 설계되어있다.
- OS 개발에서 printf 와 같은 OS가 제공하는 기능은 CPU가 일반보호예외를 발생시킨다.
- C언어로 작성 불가능한 부분은 Assembly로 작성해야한다.
