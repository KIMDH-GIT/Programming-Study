# 26-2 실습: process virtual address space
이론: [note](../../../notes/26-memory-structure/26-2-process-virtual-address-space.md)
## 실습 목적
object addresses를 관찰하되 portable ordering으로 해석하지 않는다.
## 작성할 파일
- `main.c`
## 해야 할 일
static, automatic, allocated objects의 addresses와 deterministic sum을 출력한다.
## 사용할 개념
virtual address, object pointer, `%p`, ASLR, physical address.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o address_space_app
```
## 실행 방법
```sh
./address_space_app
```
## 예상 관찰 결과
세 address와 `sum: 3`이 출력되며 numeric addresses는 고정하지 않는다.
## 확인 포인트
address 순서나 값이 아닌 정상 실행과 논리적 합만 검증한다.
## 추가 실습
- ★ 두 번 실행한다.
- ★★ virtual/physical 차이를 설명한다.
- ★★★ Linux maps를 OS 관찰로 조사한다.
## 완료 기준
valid `%p` usage로 실행하고 관찰값의 비이식성을 설명한다.
