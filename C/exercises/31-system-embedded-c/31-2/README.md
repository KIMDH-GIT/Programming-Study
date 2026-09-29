# 31-2 실습: `volatile` access와 compiler optimization
이론: [note](../../../notes/31-system-embedded-c/31-2-volatile-access-and-compiler-optimization.md)
## 실습 목적
ordinary volatile object의 read/write를 안전하게 실행하고 GCC 관찰과 C17 의미를 구분한다.
## 작성할 파일
- `main.c`
## 해야 할 일
automatic `volatile unsigned` object를 만들고 snapshot read와 write를 수행한다. arbitrary address나 실제 MMIO는 사용하지 않는다.
## 사용할 개념
volatile-qualified object, implementation-defined access, observable access, optimization boundary.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o volatile_access
```
## 실행 방법
```sh
./volatile_access
```
## 예상 관찰 결과
`snapshot=3 status=4`를 출력한다.
## 확인 포인트
volatile은 cache off, 항상 RAM, hardware register 전용 type, 특정 access width가 아니다.
## 추가 실습
- ★ 초기값을 바꾼다.
- ★★ optimization level별 assembly를 현재 host 관찰로 비교한다.
- ★★★ GCC volatile 문서와 ISO C 층을 나눠 적는다.
## 완료 기준
안전한 object만 실행하고 volatile access 의미를 hardware 보장으로 과장하지 않는다.
