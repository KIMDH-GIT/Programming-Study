# 31-8 실습: endianness와 명시적 byte 조립
이론: [note](../../../notes/31-system-embedded-c/31-8-endianness-and-explicit-byte-assembly.md)
## 실습 목적
external bytes를 host byte order와 무관하게 little/big value로 조립한다.
## 작성할 파일
- `main.c`
## 해야 할 일
`unsigned char[4]`를 받아 explicit cast/shift/OR로 LE32와 BE32를 만든다. struct pointer cast는 사용하지 않는다.
## 사용할 개념
byte order, object representation, unsigned char, safe shift.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o byte_order
```
## 실행 방법
```sh
./byte_order
```
## 예상 관찰 결과
`le=78563412 be=12345678`을 출력한다.
## 확인 포인트
endianness와 bit order를 구분한다. 현재 x86_64 결과를 “C는 little-endian”으로 일반화하지 않는다.
## 추가 실습
- ★ 16-bit 조립 함수를 만든다.
- ★★ big-endian byte 분해를 구현한다.
- ★★★ raw struct serialization과 비교한다.
## 완료 기준
host-independent exact 결과가 나오고 unsafe type punning을 사용하지 않는다.
