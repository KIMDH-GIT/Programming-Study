# 31-7 실습: bit mask·promotion·shift 경계
이론: [note](../../../notes/31-system-embedded-c/31-7-bit-mask-promotion-and-shift-boundaries.md)
## 실습 목적
valid shift만 평가하는 field mask helper를 구현한다.
## 작성할 파일
- `main.c`
## 해야 할 일
width와 shift를 검사한 뒤 `uint32_t` mask를 만든다. invalid shift는 expression 실행 전에 status 0으로 거부하고 output을 보존한다.
## 사용할 개념
integer promotion, unsigned shift, shift count, status/output.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o bit_mask
```
## 실행 방법
```sh
./bit_mask
```
## 예상 관찰 결과
`ok=1 mask=00000F00 rejected=1`을 출력한다.
## 확인 포인트
0, 32, overflow 경계를 shift 전에 검사한다. hardware count masking에 의존하지 않는다.
## 추가 실습
- ★ width 1과 32를 검사한다.
- ★★ mask overlap을 확인한다.
- ★★★ signed literal과 unsigned macro 차이를 설명한다.
## 완료 기준
정상 mask가 정확하고 invalid input에서 UB를 실행하지 않는다.
