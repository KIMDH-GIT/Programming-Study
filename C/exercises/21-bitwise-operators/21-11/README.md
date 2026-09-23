# 21-11 실습: bit test
이론: [note](../../../notes/21-bitwise-operators/21-11-bit-test.md)
## 실습 목적
AND mask로 bit 상태를 검사한다.
## 작성할 파일
`bit_test.c`
## 해야 할 일
한 bit와 여러 bits의 any/all 결과를 출력한다.
## 사용할 개념
AND mask, comparison, precedence.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic bit_test.c -o bit_test
```
## 실행 방법
```sh
./bit_test
```
## 예상 관찰 결과
설정 상태에 맞는 0/1이 출력된다.
## 확인 포인트
AND expression을 괄호로 묶는다.
## 추가 실습
- ★ clear를 검사한다.
- ★★ any/all을 비교한다.
- ★★★ helper를 만든다.
## 완료 기준
test 조건을 정확히 구분한다.
