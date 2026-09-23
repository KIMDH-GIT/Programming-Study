# 21-14 실습: `set`, `clear`, `toggle`, `read`
이론: [note](../../../notes/21-bitwise-operators/21-14-bit-operations-functions.md)
## 실습 목적
검증된 bit operations API를 작성한다.
## 작성할 파일
`bit_functions.c`
## 해야 할 일
uint8_t용 set, clear, toggle functions와 status+output-parameter read 함수를 구현한다.
## 사용할 개념
pointer parameter, mask, shift boundary, return contract.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic bit_functions.c -o bit_functions
```
## 실행 방법
```sh
./bit_functions
```
## 예상 관찰 결과
valid operations는 적용되고 invalid position·NULL output은 거부된다.
## 확인 포인트
shift 전에 position을 검사한다.
## 추가 실습
- ★ 네 함수를 연속 사용한다.
- ★★ boundaries를 시험한다.
- ★★★ NULL output 경로를 확인한다.
## 완료 기준
모든 operations가 같은 안전 contract를 따른다.
