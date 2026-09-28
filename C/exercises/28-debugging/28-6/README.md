# 28-6 실습: GDB breakpoint
이론: [note](../../../notes/28-debugging/28-6-running-gdb-and-breakpoints.md)
## 실습 목적
function breakpoint에서 program state를 관찰한다.
## 작성할 파일
- `main.c`
## 해야 할 일
`double_value`에 breakpoint를 걸고 argument를 출력한 뒤 계속 실행한다.
## 사용할 개념
GDB, breakpoint, inferior, `run`, `print`, `continue`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -g -Og main.c -o gdb_break
```
## 실행 방법
```sh
gdb ./gdb_break
```
GDB에서 `break double_value`, `run`, `print value`, `continue`를 순서대로 입력한다.
## 예상 관찰 결과
`value`는 21이며 program은 `42`를 출력한다.
## 확인 포인트
breakpoint와 C `break` statement를 구분한다.
## 추가 실습
- ★ `break main`을 추가한다.
- ★★ line breakpoint를 사용한다.
- ★★★ batch mode로 같은 관찰을 자동화한다.
## 완료 기준
breakpoint가 hit되고 argument와 최종 output을 확인한다.
