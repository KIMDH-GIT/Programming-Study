# 25-4 실습: 함수형 macro
이론: [note](../../../notes/25-preprocessor/25-4-function-like-macros.md)
## 실습 목적
function-like macro expansion과 function call을 구분한다.
## 작성할 파일
- `main.c`
## 해야 할 일
`ADD(lhs, rhs)` macro를 정의하고 side-effect-free arguments에 사용한다.
## 사용할 개념
function-like macro, macro parameter, invocation, expansion.
## 컴파일 방법
```sh
gcc -std=c17 -E main.c
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o function_macro_app
```
## 실행 방법
```sh
./function_macro_app
```
## 예상 관찰 결과
`5`가 출력된다.
## 확인 포인트
macro parameter와 function parameter를 같은 runtime object로 설명하지 않는다.
## 추가 실습
- ★ `double` literals를 사용한다.
- ★★ 같은 기능의 function과 비교한다.
- ★★★ project prefix를 설계한다.
## 완료 기준
safe input에서 expansion과 실행 결과가 정확하다.
