# 22-1 실습: 함수 주소와 함수 포인터 선언
이론: [note](../../../notes/22-function-pointers/22-1-function-address-and-pointer-declaration.md)
## 실습 목적
function과 function pointer object를 구분하고 raw declarator를 작성한다.
## 작성할 파일
`function_pointer_declaration.c`
## 해야 할 일
int 하나를 받아 int를 반환하는 `square`를 정의하고 compatible function pointer로 호출한다.
## 사용할 개념
function definition, pointer-to-function declarator, initializer.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic function_pointer_declaration.c -o function_pointer_declaration
```
## 실행 방법
```sh
./function_pointer_declaration
```
## 예상 관찰 결과
입력 5의 제곱인 `25`가 출력된다.
## 확인 포인트
`int (*fp)(int)`와 `int *fp(int)`를 구분하고 pointer를 초기화한 뒤 호출한다.
## 추가 실습
- ★ cube function으로 바꾼다.
- ★★ 두 declarator를 문장으로 읽는다.
- ★★★ array of function pointers 선언을 미리 분석한다.
## 완료 기준
warning 없이 compile·실행되고 function과 pointer object를 설명할 수 있다.
