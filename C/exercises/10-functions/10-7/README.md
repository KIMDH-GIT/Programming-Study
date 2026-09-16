# 10-7 실습: 값에 의한 전달

이론: [note](../../../notes/10-functions/10-7-pass-by-value.md)
## 실습 목적
parameter 변경과 caller 변수의 값을 구분한다.
## 작성할 파일
`pass_by_value.c`
## 해야 할 일
parameter를 두 배로 바꾸는 `show_double` 안의 값과 호출 뒤 caller 값을 출력한다.
## 사용할 개념
pass by value, argument value, parameter object, caller.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic pass_by_value.c -o pass_by_value
```
## 실행 방법
```sh
./pass_by_value
```
## 예상 관찰 결과
함수 안에서는 두 배 값, caller에서는 원래 값이 출력된다.
## 확인 포인트
parameter와 caller 변수를 별도 객체로 설명할 수 있는가?
## 추가 실습
- ★ 0 대입 - ★★ 반환값 저장 - ★★★ ABI와 언어 의미 비교
## 완료 기준
- [ ] 경고 없음 - [ ] 두 값 차이 확인 - [ ] pass by value 설명
