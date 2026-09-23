# 22-7 실습: state machine과 함수 포인터
이론: [note](../../../notes/22-function-pointers/22-7-state-machine-and-function-pointer.md)
## 실습 목적
연속 enum states와 handler table을 bounds check로 연결한다.
## 작성할 파일
`state_handler_table.c`
## 해야 할 일
IDLE·RUNNING·STOPPED handlers와 `STATE_COUNT`를 정의하고 선택 state를 dispatch한다.
## 사용할 개념
enum, no-parameter function pointer, dispatch table, range validation.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic state_handler_table.c -o state_handler_table
```
## 실행 방법
```sh
./state_handler_table
```
## 예상 관찰 결과
선택한 state 이름 또는 동작 메시지가 출력된다.
## 확인 포인트
enum의 연속성은 definition으로 확인하고 `STATE_COUNT`를 호출하지 않는다.
## 추가 실습
- ★ PAUSED state를 추가한다.
- ★★ invalid integer state를 거부한다.
- ★★★ 같은 logic을 `switch`로 비교한다.
## 완료 기준
range 안의 handler만 warning 없이 호출한다.
