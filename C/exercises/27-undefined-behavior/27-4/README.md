# 27-4 실습: signed integer overflow 방지
이론: [note](../../../notes/27-undefined-behavior/27-4-signed-integer-overflow.md)
## 실습 목적
signed arithmetic의 표현 범위를 연산 전에 검사한다.
## 작성할 파일
- `main.c`
## 해야 할 일
`checked_add`를 구현하고 `20 + 22`와 `INT_MAX + 1` 요청을 안전하게 구분한다.
## 사용할 개념
`INT_MIN`, `INT_MAX`, representability, unsigned modulo.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o checked_add
```
## 실행 방법
overflow 식 자체는 실행하지 않는다. 검사에 통과한 덧셈만 실행한다.
```sh
./checked_add
```
## 예상 관찰 결과
정상 합은 `42`, 범위 밖 요청은 실패 상태로 처리된다.
## 확인 포인트
검사가 UB 발생 후가 아니라 연산 전에 수행되는지 확인한다.
## 추가 실습
- ★ 음수 덧셈 경계를 검사한다.
- ★★ 안전한 division 함수를 만든다.
- ★★★ 안전한 shift 함수를 만든다.
## 완료 기준
경계 입력에서도 signed overflow를 평가하지 않는다.
