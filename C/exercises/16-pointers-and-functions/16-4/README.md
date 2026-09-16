# 16-4 실습: 출력 매개변수

이론: [note](../../../notes/16-pointers-and-functions/16-4-output-parameter.md)
## 실습 목적
pointer parameters로 여러 caller results를 기록한다.
## 작성할 파일
`output_parameters.c`
## 해야 할 일
17/5의 quotient와 remainder를 outputs로 기록하고 success일 때 출력한다.
## 사용할 개념
output parameter, success return, valid pointer, division boundary.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic output_parameters.c -o output_parameters
```
## 실행 방법
```sh
./output_parameters
```
## 예상 관찰 결과
quotient 3과 remainder 2가 출력된다.
## 확인 포인트
failure에서는 outputs를 읽지 않는가?
## 추가 실습
- ★ sum/difference - ★★ zero branch - ★★★ 역할표
## 완료 기준
- [ ] 경고 없음 - [ ] 3/2 출력 - [ ] contract 설명
