# 28-7 실습: `step`, `next`, `continue`
이론: [note](../../../notes/28-debugging/28-7-step-next-continue.md)
## 실습 목적
source-level execution control 명령을 구분한다.
## 작성할 파일
- `main.c`
## 해야 할 일
`add` call에서 `step`과 `next`를 각각 사용해 이동 차이를 기록한다.
## 사용할 개념
current frame, source line, call, stepping.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -g -Og main.c -o gdb_steps
```
## 실행 방법
```sh
gdb ./gdb_steps
```
## 예상 관찰 결과
`step`은 `add` 내부로, `next`는 호출 뒤 line으로 진행할 수 있고 최종 output은 `42`다.
## 확인 포인트
source step을 machine instruction 한 개라고 설명하지 않는다.
## 추가 실습
- ★ 다음 breakpoint까지 continue한다.
- ★★ finish로 caller에 돌아온다.
- ★★★ `-O2` stepping과 비교한다.
## 완료 기준
세 명령의 멈춤 조건을 실제 관찰로 설명한다.
