# 20-8 실습: alignment와 padding
이론: [note](../../../notes/20-enum-typedef-union/20-8-alignment-and-padding.md)

## 실습 목적
member order에 따른 현재 구현의 padding을 관찰한다.
## 작성할 파일
`padding_observe.c`
## 해야 할 일
member 순서가 다른 structures의 size, alignment, offsets를 출력한다.
## 사용할 개념
alignment, internal padding, trailing padding, `offsetof`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic padding_observe.c -o padding_observe
```
## 실행 방법
```sh
./padding_observe
```
## 예상 관찰 결과
현재 compiler/ABI에서 순서별 layout 차이를 관찰할 수 있다.
## 확인 포인트
특정 byte 수를 C17 보장이라고 쓰지 않는다.
## 추가 실습
- ★ 세 member 순서를 바꾼다.
- ★★ array stride를 확인한다.
- ★★★ 다른 target 결과를 조사한다.
## 완료 기준
C17 보장과 구현 관찰을 분리해 기록한다.
