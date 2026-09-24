# 25-2 실습: `#include`
이론: [note](../../../notes/25-preprocessor/25-2-include.md)
## 실습 목적
project header inclusion과 linking을 구분한다.
## 작성할 파일
- `main.c`
- `greeting.h`
## 해야 할 일
header에 `GREETING` macro를 정의하고 `main.c`에서 quote form으로 include한다.
## 사용할 개념
source inclusion, quote include, angle include, translation unit.
## 컴파일 방법
```sh
gcc -std=c17 -E main.c
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o greeting_app
```
## 실행 방법
```sh
./greeting_app
```
## 예상 관찰 결과
`hello from a header`가 출력된다.
## 확인 포인트
header를 별도 compile하거나 `.c`를 include하지 않는다.
## 추가 실습
- ★ greeting을 바꾼다.
- ★★ preprocessing 결과에서 header tokens를 찾는다.
- ★★★ search semantics를 설명한다.
## 완료 기준
header inclusion을 정확히 설명하고 warning 없이 실행한다.
