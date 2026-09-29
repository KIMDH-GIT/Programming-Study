# 31-15 실습: Part 31 종합 복습
이론: [note](../../../notes/31-system-embedded-c/31-15-part-31-review.md)
## 실습 목적
Part 31의 portability layer와 safe validation을 하나의 mock scenario로 통합한다.
## 작성할 파일
- `main.c`
## 해야 할 일
explicit byte assembly, unsigned mask, ordinary mock device, compatible callback, live context를 결합한다. 실제 MMIO address, port I/O, privileged instruction, invalid pointer, UB는 실행하지 않는다.
## 사용할 개념
abstract machine, volatile, MMIO mock, mask, endianness, function pointer, lifetime.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o part31_review
```
## 실행 방법
```sh
./part31_review
```
## 예상 관찰 결과
`control=52 event=5`를 출력한다.
## 확인 포인트
C17/compiler/ABI/OS/CPU/device 층을 구분하고 mock 결과를 real hardware 성공으로 보고하지 않는다.
## 추가 실습
- ★ 각 코드 줄에 portability layer를 붙인다.
- ★★ host-dependent observation을 portable constraint로 바꾼다.
- ★★★ 실제 target 도입 시 필요한 datasheet/ABI/compiler 검증 목록을 작성한다.
## 완료 기준
strict build/run이 통과하고 unsafe hardware access 없이 15개 Step의 핵심 계약을 설명한다.
