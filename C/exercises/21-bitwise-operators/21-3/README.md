# 21-3 실습: `^`
이론: [note](../../../notes/21-bitwise-operators/21-3-bitwise-xor.md)
## 실습 목적
XOR mask로 bits를 toggle한다.
## 작성할 파일
`bitwise_xor.c`
## 해야 할 일
같은 mask를 한 번과 두 번 적용해 결과를 출력한다.
## 사용할 개념
XOR truth table, `^=`, toggle.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic bitwise_xor.c -o bitwise_xor
```
## 실행 방법
```sh
./bitwise_xor
```
## 예상 관찰 결과
두 번째 적용 뒤 원래 flags로 돌아온다.
## 확인 포인트
XOR swap을 기본 구현으로 사용하지 않는다.
## 추가 실습
- ★ 한 bit를 toggle한다.
- ★★ mask를 조합한다.
- ★★★ swap 방법을 비교한다.
## 완료 기준
toggle 결과를 bit별로 설명한다.
