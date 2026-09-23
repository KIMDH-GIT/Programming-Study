# 21-15 실습: Part 21 종합 복습
이론: [note](../../../notes/21-bitwise-operators/21-15-part-21-review.md)
## 실습 목적
Part 21 bit operations와 safety rules를 종합한다.
## 작성할 파일
`part21_review.c`
## 해야 할 일
width-aware masks와 guards로 가상 GPIO set·clear·toggle·read와 field update를 수행한다.
## 사용할 개념
integer promotions, shifts, masks, `CHAR_BIT`, uint8_t.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic part21_review.c -o part21_review
```
## 실행 방법
```sh
./part21_review
```
## 예상 관찰 결과
각 operation 뒤 예상 flags와 test 결과가 출력된다.
## 확인 포인트
signed/invalid shifts, binary literals, `%b`를 사용하지 않는다.
## 추가 실습
- ★ operator 표를 만든다.
- ★★ behavior 분류표를 만든다.
- ★★★ API contract를 검토한다.
## 완료 기준
C17 경계를 지키며 모든 operations를 warning 없이 실행한다.
