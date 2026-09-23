# 21-2 실습: `|`
이론: [note](../../../notes/21-bitwise-operators/21-2-bitwise-or.md)
## 실습 목적
OR mask로 원하는 bits를 set한다.
## 작성할 파일
`bitwise_or.c`
## 해야 할 일
unsigned flags에 두 masks를 적용하고 각 결과를 출력한다.
## 사용할 개념
bitwise OR, `|=`, mask, hex output.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic bitwise_or.c -o bitwise_or
```
## 실행 방법
```sh
./bitwise_or
```
## 예상 관찰 결과
mask의 1 bits가 flags에 추가된다.
## 확인 포인트
logical `||`와 혼동하지 않는다.
## 추가 실습
- ★ 같은 mask를 두 번 적용한다.
- ★★ masks를 조합한다.
- ★★★ compound assignment semantics를 설명한다.
## 완료 기준
set 결과를 예측하고 warning 없이 실행한다.
