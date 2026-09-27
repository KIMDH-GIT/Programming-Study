# 27-8 실습: 초기화 상태 추적
이론: [note](../../../notes/27-undefined-behavior/27-8-uninitialized-value.md)
## 실습 목적
값이 정해진 control path에서만 object를 사용한다.
## 작성할 파일
- `main.c`
## 해야 할 일
초기값과 output parameter 성공 상태를 함께 사용한다.
## 사용할 개념
indeterminate value, unspecified value, initialization, data flow.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o initialized
```
## 실행 방법
초기화되지 않은 read는 실행하지 않는다. defined version만 실행한다.
```sh
./initialized
```
## 예상 관찰 결과
성공 path에서 `42`가 출력된다.
## 확인 포인트
모든 실제 사용 전 값이 정해졌는지 확인한다.
## 추가 실습
- ★ false path를 추가한다.
- ★★ 여러 branch의 초기화 표를 만든다.
- ★★★ warning과 C17 semantics를 분리한다.
## 완료 기준
indeterminate value를 읽는 path가 없다.
