# 27-3 실습: unspecified behavior
이론: [note](../../../notes/27-undefined-behavior/27-3-unspecified-behavior.md)
## 실습 목적
허용된 평가 순서와 unsequenced UB를 구분한다.
## 작성할 파일
- `main.c`
## 해야 할 일
서로 독립적인 `left`, `right`를 함수 인자로 전달하고 보장되는 최종 값을 설명한다.
## 사용할 개념
unspecified order, side effect, sequencing.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o unspecified
```
## 실행 방법
UB인 `use(i++, i++)`는 실행하지 않고 defined version만 실행한다.
```sh
./unspecified
```
## 예상 관찰 결과
`10 20`이 출력된다.
## 확인 포인트
인자 평가 순서는 정해지지 않아도 각 매개변수의 값은 올바르다.
## 추가 실습
- ★ 현재 평가 순서를 메시지로 관찰한다.
- ★★ side effect를 호출 전 statement로 분리한다.
- ★★★ 세 행동 범주 비교표를 만든다.
## 완료 기준
unspecified order 자체와 unsequenced UB를 구분한다.
