# 25-10 실습: Part 25 종합 복습
이론: [note](../../../notes/25-preprocessor/25-10-part-25-review.md)
## 실습 목적
include guard, safe macro, conditional configurations를 종합한다.
## 작성할 파일
- `main.c`
- `config.h`
## 해야 할 일
guarded config header를 만들고 release·debug builds에서 안전한 `SQUARE` input을 실행한다.
## 사용할 개념
header guard, object-like macro, function-like macro, `#if`, GCC `-D`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o review_release
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -DAPP_DEBUG=1 main.c -o review_debug
```
## 실행 방법
```sh
./review_release
./review_debug
```
## 예상 관찰 결과
release build는 `release`와 `16`, debug build는 `debug`와 `16`을 출력한다.
## 확인 포인트
두 configurations를 각각 compile하고 C17과 extensions를 구분한다.
## 추가 실습
- ★ 다른 safe square input을 사용한다.
- ★★ debug preprocessing 결과를 본다.
- ★★★ mechanism 선택표를 만든다.
## 완료 기준
두 strict C17 builds와 runs가 모두 성공하고 Part 25 용어를 정확히 설명한다.
