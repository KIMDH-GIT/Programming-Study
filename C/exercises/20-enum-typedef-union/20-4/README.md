# 20-4 실습: `switch` 기반 state machine
이론: [note](../../../notes/20-enum-typedef-union/20-4-switch-based-state-machine.md)

## 실습 목적
enum state transition을 switch로 구현한다.
## 작성할 파일
`state_machine.c`
## 해야 할 일
IDLE→RUNNING→DONE 정상 전이와 ERROR/default 경로를 구현한다.
## 사용할 개념
enum, switch, case, default, pass-by-value.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic state_machine.c -o state_machine
```
## 실행 방법
```sh
./state_machine
```
## 예상 관찰 결과
두 번 전이한 상태가 DONE임을 확인한다.
## 확인 포인트
의도하지 않은 fallthrough와 duplicate case가 없다.
## 추가 실습
- ★ ERROR를 흡수 상태로 만든다.
- ★★ event를 추가한다.
- ★★★ transition table과 비교한다.
## 완료 기준
모든 state 경로가 warning 없이 명시적으로 처리된다.
