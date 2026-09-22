# 20-10 실습: endianness 기초
이론: [note](../../../notes/20-enum-typedef-union/20-10-endianness-basics.md)

## 실습 목적
native multi-byte representation의 byte order를 관찰한다.
## 작성할 파일
`endian_observe.c`
## 해야 할 일
`UINT16_MAX`가 정의되고 `CHAR_BIT == 8`일 때만 `uint16_t`의 두 bytes를 낮은 주소부터 출력한다. 다른 구현은 전제 불충족을 출력한다.
## 사용할 개념
byte order, `uint16_t`, `UINT16_MAX`, `CHAR_BIT`, object representation.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic endian_observe.c -o endian_observe
```
## 실행 방법
```sh
./endian_observe
```
## 예상 관찰 결과
전제가 맞으면 두 bytes와 little-/big-/other 분류가, 아니면 전제 불충족 안내가 출력된다.
## 확인 포인트
`bytes[1]` 접근 전에 8-bit byte 전제를 compile-time condition으로 확인한다.
## 추가 실습
- ★ 여러 values를 관찰한다.
- ★★ 표준/구현 규칙을 분류한다.
- ★★★ protocol order와 native order 차이를 조사한다.
## 완료 기준
byte order를 warning 없이 관찰하고 결과를 구현 특성으로 설명한다.
