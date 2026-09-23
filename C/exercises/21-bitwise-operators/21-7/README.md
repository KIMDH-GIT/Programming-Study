# 21-7 실습: 폭에 맞는 unsigned bit mask
이론: [note](../../../notes/21-bitwise-operators/21-7-width-aware-unsigned-mask.md)
## 실습 목적
valid position에서 single-bit mask를 만든다.
## 작성할 파일
`width_mask.c`
## 해야 할 일
`UINT_MAX`로 unsigned int value bits를 계산하고 position 검사 뒤 mask를 출력한다.
## 사용할 개념
`UINT_MAX`, unsigned right shift, `1u`, shift boundary.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic width_mask.c -o width_mask
```
## 실행 방법
```sh
./width_mask
```
## 예상 관찰 결과
single-bit mask와 현재 width가 출력된다.
## 확인 포인트
32-bit나 shift modulo를 가정하지 않는다.
## 추가 실습
- ★ position을 바꾼다.
- ★★ invalid input을 거부한다.
- ★★★ padding bits를 조사한다.
## 완료 기준
경계 검사 후 warning 없이 mask를 만든다.
