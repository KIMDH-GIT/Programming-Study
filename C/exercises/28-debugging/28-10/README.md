# 28-10 실습: `watch`
이론: [note](../../../notes/28-debugging/28-10-watch.md)
## 실습 목적
값이 변경되는 정확한 execution point를 찾는다.
## 작성할 파일
- `main.c`
## 해야 할 일
`total` initialization 뒤 watchpoint를 설정하고 변경을 계속 관찰한다.
## 사용할 개념
watchpoint, expression, scope, hardware/software implementation.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -g -Og main.c -o gdb_watch
```
## 실행 방법
```sh
gdb ./gdb_watch
```
## 예상 관찰 결과
`total`이 0→1→3→6으로 변경되고 program은 `6`을 출력한다.
## 확인 포인트
local object의 scope와 watchpoint lifetime을 함께 본다.
## 추가 실습
- ★ watchpoint 목록을 확인한다.
- ★★ 배열 원소를 감시한다.
- ★★★ target별 watchpoint 제한을 조사한다.
## 완료 기준
변경 지점과 old/new value를 확인한다.
