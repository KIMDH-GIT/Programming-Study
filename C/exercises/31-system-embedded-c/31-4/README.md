# 31-4 실습: memory-mapped I/O safe mock
이론: [note](../../../notes/31-system-embedded-c/31-4-memory-mapped-io.md)
## 실습 목적
MMIO interface 모양을 ordinary object로 검증하고 실제 hardware access와 구분한다.
## 작성할 파일
- `main.c`
## 해야 할 일
volatile member를 가진 `MockDevice`를 만들고 control write/status read를 수행한다. 실제 MMIO 주소, arbitrary pointer cast, port I/O, privileged instruction은 절대 실행하지 않는다.
## 사용할 개념
volatile member, safe mock, integer-pointer portability, C/compiler/CPU/device 층.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o mmio_mock
```
## 실행 방법
```sh
./mmio_mock
```
## 예상 관찰 결과
`control=1 status=7`을 출력한다. 이는 ordinary mock 결과다.
## 확인 포인트
실제 address와 register side effect를 검증했다고 보고하지 않는다. `0x40000000` 같은 임의 주소를 host에서 역참조하지 않는다.
## 추가 실습
- ★ mock register를 하나 추가한다.
- ★★ MMIO의 여섯 층을 표로 만든다.
- ★★★ datasheet 기반 contract를 실행 없이 분석한다.
## 완료 기준
safe mock만 실행하고 unsafe real hardware access는 실행하지 않는다.
