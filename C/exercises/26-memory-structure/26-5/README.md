# 26-5 실습: stack과 자동 객체
이론: [note](../../../notes/26-memory-structure/26-5-stack-and-automatic-objects.md)
## 실습 목적
automatic storage duration과 stack implementation을 구분한다.
## 작성할 파일
- `main.c`
## 해야 할 일
parameter와 local object를 사용하는 function을 작성하고 결과를 확인한다.
## 사용할 개념
automatic storage duration, block scope, lifetime, stack frame, register allocation.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o automatic_app
```
## 실행 방법
```sh
./automatic_app
```
## 예상 관찰 결과
`10`이 출력된다.
## 확인 포인트
local과 parameter를 반드시 stack에 있다고 설명하지 않는다.
## 추가 실습
- ★ local 계산을 추가한다.
- ★★ optimization을 설명한다.
- ★★★ dangling pointer를 분석한다.
## 완료 기준
정상 실행과 automatic/stack 구분을 모두 설명한다.
