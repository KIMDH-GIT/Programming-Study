# 21-1 실습: `&`
이론: [note](../../../notes/21-bitwise-operators/21-1-bitwise-and.md)
## 실습 목적
binary AND로 선택한 bits를 검사한다.
## 작성할 파일
`bitwise_and.c`
## 해야 할 일
unsigned flags와 mask에 `&`를 적용해 hex 결과와 boolean 검사를 출력한다.
## 사용할 개념
bitwise AND, mask, hex literal, comparison.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic bitwise_and.c -o bitwise_and
```
## 실행 방법
```sh
./bitwise_and
```
## 예상 관찰 결과
mask와 겹친 bits만 남는다.
## 확인 포인트
주소 `&x`, logical `&&`, binary `&`를 구분한다.
## 추가 실습
- ★ mask를 바꾼다.
- ★★ logical 결과와 비교한다.
- ★★★ promotion을 분석한다.
## 완료 기준
AND 결과를 bit pattern으로 설명하고 warning 없이 실행한다.
