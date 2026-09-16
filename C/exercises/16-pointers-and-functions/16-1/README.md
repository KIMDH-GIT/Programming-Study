# 16-1 실습: 값 매개변수와 원본

이론: [note](../../../notes/16-pointers-and-functions/16-1-value-parameter-original.md)
## 실습 목적
caller object와 value parameter object가 별도임을 확인한다.
## 작성할 파일
`value_parameter.c`
## 해야 할 일
parameter를 100으로 바꾸고 callee·caller 값을 각각 출력한다.
## 사용할 개념
argument value, parameter object, caller object, pass-by-value.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic value_parameter.c -o value_parameter
```
## 실행 방법
```sh
./value_parameter
```
## 예상 관찰 결과
parameter는 100, caller는 원래 10이다.
## 확인 포인트
두 values를 별도 objects의 상태로 설명하는가?
## 추가 실습
- ★ zero assignment - ★★ same names - ★★★ diagram
## 완료 기준
- [ ] 경고 없이 컴파일 - [ ] 100/10 출력 - [ ] pass-by-value 설명
