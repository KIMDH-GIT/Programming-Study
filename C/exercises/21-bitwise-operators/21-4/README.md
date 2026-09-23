# 21-4 실습: `~`와 integer promotion
이론: [note](../../../notes/21-bitwise-operators/21-4-bitwise-not-and-promotion.md)
## 실습 목적
NOT이 promoted width 전체에 적용됨을 확인한다.
## 작성할 파일
`bitwise_not.c`
## 해야 할 일
unsigned value, NOT 결과, unsigned int bit width를 출력한다.
## 사용할 개념
`~`, integer promotion, `CHAR_BIT`, `sizeof`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic bitwise_not.c -o bitwise_not
```
## 실행 방법
```sh
./bitwise_not
```
## 예상 관찰 결과
현재 unsigned int width에 맞는 반전 결과가 출력된다.
## 확인 포인트
결과를 8-bit `0xF0`으로 고정하지 않는다.
## 추가 실습
- ★ mask를 바꾼다.
- ★★ narrow type promotion을 분석한다.
- ★★★ signed 결과를 실행 없이 비교한다.
## 완료 기준
promotion과 width를 포함해 결과를 설명한다.
