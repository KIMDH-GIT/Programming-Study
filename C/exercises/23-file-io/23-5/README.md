# 23-5 실습: `fgets`와 `fputs`
이론: [note](../../../notes/23-file-io/23-5-fgets-and-fputs.md)
## 실습 목적
bounded line I/O와 newline preservation을 확인한다.
## 작성할 파일
`line_file_io.c`
## 해야 할 일
두 lines를 쓰고 첫 line을 fixed buffer로 읽는다.
## 사용할 개념
`fgets`, `fputs`, capacity, newline, null terminator.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic line_file_io.c -o line_file_io
```
## 실행 방법
```sh
./line_file_io
```
## 예상 관찰 결과
`read: first line`이 한 line으로 출력된다.
## 확인 포인트
`fgets`가 newline을 자동 제거하거나 line 전체를 항상 보장한다고 가정하지 않는다.
## 추가 실습
- ★ newline을 찾는다.
- ★★ 작은 buffer로 long line을 나눈다.
- ★★★ remainder 처리 logic을 작성한다.
## 완료 기준
return과 capacity를 검사해 warning 없이 한 line을 읽는다.
