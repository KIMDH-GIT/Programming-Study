# 21-10 실습: bit toggle
이론: [note](../../../notes/21-bitwise-operators/21-10-bit-toggle.md)
## 실습 목적
XOR mask로 selected bits를 반전한다.
## 작성할 파일
`bit_toggle.c`
## 해야 할 일
flags에 mask를 적용해 toggle 결과를 출력한다.
## 사용할 개념
`^=`, mask, bit position.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic bit_toggle.c -o bit_toggle
```
## 실행 방법
```sh
./bit_toggle
```
## 예상 관찰 결과
mask positions만 반전된다.
## 확인 포인트
endianness와 integer bit positions를 구분한다.
## 추가 실습
- ★ 두 번 적용한다.
- ★★ 세 operations를 비교한다.
- ★★★ byte order와 구분한다.
## 완료 기준
target bits만 정확히 toggle한다.
