# 26-10 실습: Part 26 종합 복습
이론: [note](../../../notes/26-memory-structure/26-10-part-26-review.md)
## 실습 목적
C17 object semantics와 implementation layout을 종합한다.
## 작성할 파일
- `main.c`
## 해야 할 일
static, automatic, allocated objects를 안전하게 사용하고 address 하나를 관찰한다.
## 사용할 개념
scope, linkage, storage duration, lifetime, `%p`, implementation layout.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o review_app
```
## 실행 방법
```sh
./review_app
```
## 예상 관찰 결과
`0 2`와 allocated address가 출력되며 address는 고정하지 않는다.
## 확인 포인트
C17 guarantees와 compiler/linker/OS observations를 분리한다.
## 추가 실습
- ★ 호출을 늘린다.
- ★★ 두 종류 표를 만든다.
- ★★★ hosted/embedded를 비교한다.
## 완료 기준
strict C17 실행과 four-property 분석을 모두 완료한다.
