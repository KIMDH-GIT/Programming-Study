# 26-9 실습: OS·ABI·최적화·ASLR에 따른 차이
이론: [note](../../../notes/26-memory-structure/26-9-os-abi-optimization-and-aslr.md)
## 실습 목적
portable values와 implementation-dependent addresses를 분리한다.
## 작성할 파일
- `main.c`
## 해야 할 일
같은 executable을 두 번 실행해 value와 address를 관찰한다.
## 사용할 개념
optimization, ABI, PIE, ASLR, virtual address.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o environment_app
```
## 실행 방법
```sh
./environment_app
./environment_app
```
## 예상 관찰 결과
두 실행 모두 value는 6이며 address는 고정하지 않는다.
## 확인 포인트
PIE·ASLR을 끄지 않고 addresses를 PASS 조건으로 사용하지 않는다.
## 추가 실습
- ★ values만 비교한다.
- ★★ optimized build를 비교한다.
- ★★★ 책임 표를 만든다.
## 완료 기준
portable behavior와 environment observation을 구분한다.
