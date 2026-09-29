# 31-12 실습: driver interface의 const correctness
이론: [note](../../../notes/31-system-embedded-c/31-12-driver-interface-const-correctness.md)
## 실습 목적
read-only와 mutable driver operation을 pointer qualifier로 구분한다.
## 작성할 파일
- `main.c`
## 해야 할 일
`read_status(const Device *)`와 `write_control(Device *, unsigned)`를 작성한다. 모두 non-NULL live object를 precondition으로 받으며 arbitrary MMIO address는 사용하지 않는다.
## 사용할 개념
const pointer, volatile member, mutation surface, safe mock.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o const_driver
```
## 실행 방법
```sh
./const_driver
```
## 예상 관찰 결과
`control=3 status=9`를 출력한다.
## 확인 포인트
const는 ROM, synchronization, deep const를 뜻하지 않는다. read function은 cast로 const를 제거하지 않는다.
## 추가 실습
- ★ getter를 하나 더 만든다.
- ★★ read-only configuration view를 설계한다.
- ★★★ const-removing cast의 계약 위반을 설명한다.
## 완료 기준
signature가 mutation 의도를 표현하고 mock 실행이 정확하다.
