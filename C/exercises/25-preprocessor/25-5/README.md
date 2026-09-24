# 25-5 실습: `SQUARE(x)`와 괄호
이론: [note](../../../notes/25-preprocessor/25-5-square-and-parentheses.md)
## 실습 목적
macro expansion의 precedence 문제를 괄호로 방지한다.
## 작성할 파일
- `main.c`
## 해야 할 일
올바르게 괄호 처리한 `SQUARE`로 `SQUARE(1 + 2)`를 계산한다.
## 사용할 개념
macro expansion, operator precedence, argument parentheses, replacement parentheses.
## 컴파일 방법
```sh
gcc -std=c17 -E main.c
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o square_app
```
## 실행 방법
```sh
./square_app
```
## 예상 관찰 결과
`9`가 출력된다.
## 확인 포인트
side-effect-free argument만 실행하고 괄호가 multiple evaluation을 막지는 않음을 설명한다.
## 추가 실습
- ★ 다른 addition expression을 제곱한다.
- ★★ surrounding multiplication과 조합한다.
- ★★★ typed function과 비교한다.
## 완료 기준
expansion 구조를 설명하고 strict C17 실행 결과가 9다.
