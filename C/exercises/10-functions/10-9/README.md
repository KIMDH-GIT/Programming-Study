# 10-9 실습: 계산기 연산 함수 분리

이론: [note](../../../notes/10-functions/10-9-calculator-functions.md)
## 실습 목적
계산기 연산을 역할별 함수로 분리한다.
## 작성할 파일
`calculator_functions.c`
## 해야 할 일
`add`, `subtract`, `multiply`를 정의하고 8.0과 3.0으로 각각 호출해 결과를 출력한다.
## 사용할 개념
function decomposition, `double` parameters, return value, arithmetic operators.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic calculator_functions.c -o calculator_functions
```
## 실행 방법
```sh
./calculator_functions
```
## 예상 관찰 결과
11.0, 5.0, 24.0이 순서대로 출력된다.
## 확인 포인트
각 함수가 하나의 연산만 수행하는가?
## 추가 실습
- ★ 세 연산 출력 - ★★ 안전한 나눗셈 호출 - ★★★ `switch` 선택
## 완료 기준
- [ ] 경고 없음 - [ ] 세 결과 정확 - [ ] 역할별 함수 분리
