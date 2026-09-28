# 28-9 실습: `backtrace`
이론: [note](../../../notes/28-debugging/28-9-backtrace.md)
## 실습 목적
현재 frame에서 caller chain을 읽는다.
## 작성할 파일
- `main.c`
## 해야 할 일
`leaf` breakpoint에서 `bt`, `frame 1`, `info args`를 실행한다.
## 사용할 개념
frame, caller, argument, source location, optimization.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -g -Og main.c -o gdb_backtrace
```
## 실행 방법
```sh
gdb ./gdb_backtrace
```
## 예상 관찰 결과
`leaf`·`middle`·`top`·`main` caller chain과 최종 output `42`를 확인한다.
## 확인 포인트
frame 0부터 바깥 caller 방향으로 읽는다.
## 추가 실습
- ★ frame arguments를 기록한다.
- ★★ `bt 2`를 사용한다.
- ★★★ optimized build와 비교한다.
## 완료 기준
호출 경로와 각 frame의 역할을 설명한다.
