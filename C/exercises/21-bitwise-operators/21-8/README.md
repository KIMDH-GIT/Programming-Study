# 21-8 실습: bit set
이론: [note](../../../notes/21-bitwise-operators/21-8-bit-set.md)
## 실습 목적
OR mask로 independent flags를 set한다.
## 작성할 파일
`bit_set.c`
## 해야 할 일
READ에서 시작해 WRITE를 set하고 결과를 출력한다.
## 사용할 개념
enum flags, mask, `|=`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic bit_set.c -o bit_set
```
## 실행 방법
```sh
./bit_set
```
## 예상 관찰 결과
두 flags의 bits가 함께 set된다.
## 확인 포인트
state enum과 bit flags를 구분한다.
## 추가 실습
- ★ flag를 추가한다.
- ★★ 두 번 set한다.
- ★★★ 모델 차이를 설명한다.
## 완료 기준
다른 bits를 보존하며 정확히 set한다.
