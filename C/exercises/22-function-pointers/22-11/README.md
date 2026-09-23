# 22-11 실습: Part 22 종합 복습
이론: [note](../../../notes/22-function-pointers/22-11-part-22-review.md)
## 실습 목적
function pointer 선언·typedef·dispatch·bounds·null checks를 종합한다.
## 작성할 파일
`part22_review.c`
## 해야 할 일
이름과 operation pointer를 가진 table을 만들고 valid index의 arithmetic operation을 호출한다.
## 사용할 개념
typedef function pointer, struct, function pointer array, bounds check, null check.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic part22_review.c -o part22_review
```
## 실행 방법
```sh
./part22_review
```
## 예상 관찰 결과
선택한 operation 이름과 정확한 계산 결과가 출력된다.
## 확인 포인트
incompatible cast, function pointer arithmetic, `%p` 출력, out-of-bounds call을 사용하지 않는다.
## 추가 실습
- ★ 모든 entries를 순회한다.
- ★★ enum index를 추가한다.
- ★★★ driver interface와 callback까지 결합한다.
## 완료 기준
Part 22 핵심 규칙을 설명하고 warning 없이 안전하게 dispatch한다.
