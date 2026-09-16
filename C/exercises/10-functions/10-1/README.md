# 10-1 실습: 함수 정의와 호출

이론: [note](../../../notes/10-functions/10-1-function-definition-call.md)
## 실습 목적
함수 정의의 구성 요소와 호출 결과를 확인한다.
## 작성할 파일
`function_call.c`
## 해야 할 일
두 `int`를 더해 반환하는 `add`를 정의하고 `main`에서 `add(3, 4)`를 호출해 결과를 출력한다.
## 사용할 개념
return type, function name, parameter list, function body, call, argument.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic function_call.c -o function_call
```
## 실행 방법
```sh
./function_call
```
## 예상 관찰 결과
`7`이 출력된다.
## 확인 포인트
정의·호출·반환값과 parameter·argument를 각각 구분했는가?
## 추가 실습
- ★ 곱 함수 - ★★ 세 수의 합 - ★★★ 구성 요소에 주석 달기
## 완료 기준
- [ ] 경고 없이 컴파일 - [ ] 결과 7 - [ ] 각 구성 요소 설명
