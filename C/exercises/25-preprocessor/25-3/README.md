# 25-3 실습: 객체형 `#define`
이론: [note](../../../notes/25-preprocessor/25-3-object-like-define.md)
## 실습 목적
object-like macro expansion과 C object를 구분한다.
## 작성할 파일
- `main.c`
## 해야 할 일
`APP_NAME`, `BUFFER_SIZE` macros를 정의해 배열과 출력에 사용한다.
## 사용할 개념
macro name, replacement list, preprocessing token, array bound.
## 컴파일 방법
```sh
gcc -std=c17 -E main.c
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o object_macro_app
```
## 실행 방법
```sh
./object_macro_app
```
## 예상 관찰 결과
`preprocessor study: 4 elements`가 출력된다.
## 확인 포인트
macro를 typed variable이나 storage object로 설명하지 않는다.
## 추가 실습
- ★ 배열 크기를 5로 바꾼다.
- ★★ enum constant와 비교한다.
- ★★★ const object와 비교한다.
## 완료 기준
expansion 결과를 관찰하고 strict C17 build·실행이 성공한다.
