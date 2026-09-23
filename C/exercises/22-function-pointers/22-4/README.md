# 22-4 실습: `typedef` 함수 포인터
이론: [note](../../../notes/22-function-pointers/22-4-typedef-function-pointer.md)
## 실습 목적
raw declarator를 typedef name으로 바꾸고 underlying type을 설명한다.
## 작성할 파일
`typedef_function_pointer.c`
## 해야 할 일
`BinaryOperation` pointer typedef를 만들고 multiply를 calculator에 전달한다.
## 사용할 개념
typedef, pointer-to-function type, compatible callback parameter.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic typedef_function_pointer.c -o typedef_function_pointer
```
## 실행 방법
```sh
./typedef_function_pointer
```
## 예상 관찰 결과
6과 7의 곱 `42`가 출력된다.
## 확인 포인트
typedef가 function이나 object를 생성하지 않고 기존 pointer type에 이름을 붙임을 설명한다.
## 추가 실습
- ★ add target으로 바꾼다.
- ★★ raw 선언과 typedef 선언을 나란히 쓴다.
- ★★★ function type typedef와 차이를 분석한다.
## 완료 기준
typedef를 펼친 raw type을 읽고 warning 없이 실행한다.
