# 20-11 실습: Part 20 종합 복습
이론: [note](../../../notes/20-enum-typedef-union/20-11-part-20-review.md)

## 실습 목적
enum, typedef, union, layout와 byte-order 규칙을 종합한다.
## 작성할 파일
`part20_review.c`
## 해야 할 일
tagged union 두 variants를 생성·출력하고 layout과 native byte order를 관찰한다.
## 사용할 개념
enum, typedef, union, switch, `sizeof`, `_Alignof`, `offsetof`, byte order.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic part20_review.c -o part20_review
```
## 실행 방법
```sh
./part20_review
```
## 예상 관찰 결과
matching variants와 현재 구현의 layout·byte-order 관찰 결과가 출력된다.
## 확인 포인트
inactive member, raw equality, raw serialization에 의존하지 않는다.
## 추가 실습
- ★ 용어 표를 만든다.
- ★★ 표준/구현 규칙을 분류한다.
- ★★★ API invariant를 문서화한다.
## 완료 기준
Part 20 핵심 구분을 설명하고 warning 없이 실행한다.
