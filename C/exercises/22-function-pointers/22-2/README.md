# 22-2 실습: 함수 포인터 대입과 호출
이론: [note](../../../notes/22-function-pointers/22-2-assignment-and-call.md)
## 실습 목적
`f`·`&f` 대입과 `fp()`·`(*fp)()` 호출 관계를 확인한다.
## 작성할 파일
`function_pointer_call.c`
## 해야 할 일
add를 function pointer에 두 방식으로 대입하고 두 호출 형태의 결과를 출력한다.
## 사용할 개념
function designator conversion, unary `&`, function call operator.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic function_pointer_call.c -o function_pointer_call
```
## 실행 방법
```sh
./function_pointer_call
```
## 예상 관찰 결과
두 방식 모두 선택한 add function의 올바른 합을 출력한다.
## 확인 포인트
변환 전 function type과 pointer-to-function type을 같다고 설명하지 않는다.
## 추가 실습
- ★ subtract로 target을 바꾼다.
- ★★ direct call 결과와 비교한다.
- ★★★ conversion이 억제되는 context를 적는다.
## 완료 기준
두 assignment와 두 call이 warning 없이 동작하고 의미를 구분해 설명한다.
