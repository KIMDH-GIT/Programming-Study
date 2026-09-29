# 31-5 실습: register read-modify-write
이론: [note](../../../notes/31-system-embedded-c/31-5-register-read-modify-write-and-hardware-contracts.md)
## 실습 목적
field mask transformation을 검증하고 hardware-specific RMW 위험을 분리한다.
## 작성할 파일
- `main.c`
## 해야 할 일
old value, field mask, field value를 받는 pure helper를 작성한다. 실제 register는 읽거나 쓰지 않는다. W1C와 reserved-bit 규약은 분석 항목으로만 다룬다.
## 사용할 개념
bit mask, read-modify-write, W1C, device contract, pure function.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o register_rmw
```
## 실행 방법
```sh
./register_rmw
```
## 예상 관찰 결과
`before=A5 after=A3`을 출력한다.
## 확인 포인트
변경 대상 field 외 bit가 유지된다. C expression을 instruction 하나로 정의하지 않는다.
## 추가 실습
- ★ 다른 low nibble 값을 넣는다.
- ★★ 두 field mask의 비겹침을 확인한다.
- ★★★ W1C에서 `|=`가 위험한 이유를 sequence로 적는다.
## 완료 기준
mask 결과가 정확하고 device contract 없이는 실제 RMW 안전성을 주장하지 않는다.
