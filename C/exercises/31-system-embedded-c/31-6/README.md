# 31-6 실습: fixed-width integer 제공 여부
이론: [note](../../../notes/31-system-embedded-c/31-6-fixed-width-integer-availability.md)
## 실습 목적
exact-width type의 optional 제공 여부와 현재 host observation을 구분한다.
## 작성할 파일
- `main.c`
## 해야 할 일
`UINT32_MAX` macro 존재 여부로 분기하고 available/unavailable 결과를 출력한다. 현재 결과를 모든 C implementation 보장으로 일반화하지 않는다.
## 사용할 개념
`<stdint.h>`, exact-width type, `CHAR_BIT`, host observation.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o fixed_width
```
## 실행 방법
```sh
./fixed_width
```
## 예상 관찰 결과
현재 host에서는 `uint32_t=available size=4 max_match=1`이다. conforming target에서는 unavailable도 valid하다.
## 확인 포인트
`uint32_t` 존재와 CPU register/bus width를 연결하지 않는다. C byte를 항상 8 bit라고 하지 않는다.
## 추가 실습
- ★ `UINT16_MAX`도 확인한다.
- ★★ `CHAR_BIT`를 함께 출력한다.
- ★★★ exact/least/fast type 차이를 정리한다.
## 완료 기준
조건부 제공을 처리하고 host 결과와 C17 보장을 분리한다.
