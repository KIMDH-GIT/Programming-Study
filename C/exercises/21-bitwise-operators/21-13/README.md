# 21-13 실습: `uint8_t GPIO` 가상 레지스터
이론: [note](../../../notes/21-bitwise-operators/21-13-uint8-gpio-register.md)
## 실습 목적
8-bit 일반 object를 register-like value로 조작한다.
## 작성할 파일
`virtual_gpio.c`
## 해야 할 일
pin masks로 set·clear·test하고 hex로 출력한다.
## 사용할 개념
`uint8_t`, integer promotion, masks.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic virtual_gpio.c -o virtual_gpio
```
## 실행 방법
```sh
./virtual_gpio
```
## 예상 관찰 결과
선택 pins 상태가 8-bit hex로 출력된다.
## 확인 포인트
실제 MMIO라고 설명하지 않는다.
## 추가 실습
- ★ 두 pins를 set한다.
- ★★ clear를 추가한다.
- ★★★ hardware 차이를 조사한다.
## 완료 기준
promotion을 고려해 warning 없이 실행한다.
