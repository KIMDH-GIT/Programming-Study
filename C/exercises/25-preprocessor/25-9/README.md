# 25-9 실습: macro·함수·상수 선택
이론: [note](../../../notes/25-preprocessor/25-9-choosing-macros-functions-and-constants.md)
## 실습 목적
preprocessing과 typed C mechanisms를 목적에 맞게 선택한다.
## 작성할 파일
- `main.c`
## 해야 할 일
macro, enum constant, `const` object, function을 각각 적절한 역할에 사용한다.
## 사용할 개념
conditional macro, enum constant, typed object, typed function.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o choice_app
```
## 실행 방법
```sh
./choice_app
```
## 예상 관찰 결과
`9 1.5 4`가 출력된다.
## 확인 포인트
모든 named value와 계산을 macro로 만들지 않는다.
## 추가 실습
- ★ buffer size를 바꾼다.
- ★★ function argument 평가 횟수를 설명한다.
- ★★★ 선택 guideline을 작성한다.
## 완료 기준
각 mechanism의 역할을 설명하고 warning 없이 실행한다.
