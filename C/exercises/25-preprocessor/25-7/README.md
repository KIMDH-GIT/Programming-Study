# 25-7 실습: conditional directives
이론: [note](../../../notes/25-preprocessor/25-7-conditional-directives.md)
## 실습 목적
compile-time configurations와 runtime branch를 구분한다.
## 작성할 파일
- `main.c`
## 해야 할 일
default build와 `DEBUG`, `FEATURE_LEVEL=2` build를 각각 만든다.
## 사용할 개념
`#if`, `#ifdef`, `#ifndef`, `#endif`, `defined`, GCC `-D`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o app_release
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -DDEBUG -DFEATURE_LEVEL=2 main.c -o app_debug
```
## 실행 방법
```sh
./app_release
./app_debug
```
## 예상 관찰 결과
각각 `release basic`, `debug advanced`가 출력된다.
## 확인 포인트
두 configurations를 독립적으로 build하고 `-D`를 driver option으로 설명한다.
## 추가 실습
- ★ feature level을 3으로 바꾼다.
- ★★ `defined(DEBUG)` form을 사용한다.
- ★★★ runtime flag와 비교한다.
## 완료 기준
두 configuration의 strict C17 build·실행이 모두 성공한다.
