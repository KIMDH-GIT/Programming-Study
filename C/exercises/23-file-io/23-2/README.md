# 23-2 실습: 파일 모드와 열기 실패
이론: [note](../../../notes/23-file-io/23-2-file-modes-and-open-failure.md)
## 실습 목적
`"w"` truncation과 `"a"` append behavior를 구분한다.
## 작성할 파일
`file_modes.c`
## 해야 할 일
학습용 file에 첫 line을 새로 쓰고 두 번째 line을 append한다.
## 사용할 개념
open modes, create, truncate, append, failure handling.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic file_modes.c -o file_modes
```
## 실행 방법
```sh
./file_modes
```
## 예상 관찰 결과
file에 두 lines가 순서대로 남는다.
## 확인 포인트
중요한 기존 file을 사용하지 않고 update-stream sequencing도 설명한다.
## 추가 실습
- ★ `"r"` failure를 확인한다.
- ★★ modes 표를 직접 작성한다.
- ★★★ update stream direction rules를 정리한다.
## 완료 기준
mode별 existence·truncate·append 차이를 설명하고 warning 없이 실행한다.
