# 13-5 실습: 제한된 문자열 입력과 잘린 줄 처리

이론: [note](../../../notes/13-characters-and-strings/13-5-bounded-input-truncated-line.md)
## 실습 목적
크기가 제한된 line input과 residue 처리를 연습한다.
## 작성할 파일
`bounded_line_input.c`
## 해야 할 일
크기 8 buffer에 `fgets`로 입력하고 newline을 제거하거나 남은 줄을 버린다.
## 사용할 개념
`fgets`, return value, buffer size, newline, EOF, truncation.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic bounded_line_input.c -o bounded_line_input
```
## 실행 방법
```sh
./bounded_line_input
```
## 예상 관찰 결과
짧거나 긴 입력 모두 저장 가능한 부분만 null-terminated string으로 출력된다.
## 확인 포인트
실패한 입력을 출력하지 않고 잘린 residue를 제거하는가?
## 추가 실습
- ★ 짧은 줄 - ★★ 긴 줄 - ★★★ 두 줄 연속
## 완료 기준
- [ ] 경고 없음 - [ ] 반환값 확인 - [ ] newline/residue 처리
