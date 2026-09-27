# 27-10 실습: optimization과 UB 분리
이론: [note](../../../notes/27-undefined-behavior/27-10-compiler-optimization-and-ub.md)
## 실습 목적
defined program의 output과 generated code 관찰을 분리한다.
## 작성할 파일
- `main.c`
## 해야 할 일
증가 전 범위를 검사하고 `-O0`, `-O2` build를 각각 실행한다.
## 사용할 개념
optimizer assumption, signed overflow, implementation option.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -O0 main.c -o app_o0
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -O2 main.c -o app_o2
```
## 실행 방법
UB source는 실행하지 않는다. 두 defined binary만 실행한다.
```sh
./app_o0
./app_o2
```
## 예상 관찰 결과
두 실행 모두 `42`를 출력한다.
## 확인 포인트
assembly 차이를 C17 요구사항이라고 설명하지 않는다.
## 추가 실습
- ★ binary size를 비교한다.
- ★★ `gcc -S`를 현재 GCC 관찰로 기록한다.
- ★★★ range 정보가 optimizer에 주는 의미를 설명한다.
## 완료 기준
C17 결과와 optimization 관찰을 별도로 보고한다.
