# 21-9 실습: bit clear
이론: [note](../../../notes/21-bitwise-operators/21-9-bit-clear.md)
## 실습 목적
AND와 NOT mask로 특정 bit를 clear한다.
## 작성할 파일
`bit_clear.c`
## 해야 할 일
unsigned flags에서 한 bit를 clear하고 결과를 출력한다.
## 사용할 개념
`&=`, `~`, unsigned mask, width.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic bit_clear.c -o bit_clear
```
## 실행 방법
```sh
./bit_clear
```
## 예상 관찰 결과
target bit만 0이 된다.
## 확인 포인트
mask와 flags를 같은 unsigned type으로 둔다.
## 추가 실습
- ★ 두 bits를 clear한다.
- ★★ idempotence를 확인한다.
- ★★★ signed mask 문제를 분석한다.
## 완료 기준
target 외 bits를 보존해 clear한다.
