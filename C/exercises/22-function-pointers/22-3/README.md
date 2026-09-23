# 22-3 실습: 함수 포인터 타입 호환성
이론: [note](../../../notes/22-function-pointers/22-3-type-compatibility.md)
## 실습 목적
compatible signature와 null function pointer 검사를 적용한다.
## 작성할 파일
`compatible_function_pointer.c`
## 해야 할 일
동일한 signature의 add와 subtract 중 하나를 대입하고 null check 뒤 호출한다.
## 사용할 개념
compatible function type, null function pointer, equality comparison.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic compatible_function_pointer.c -o compatible_function_pointer
```
## 실행 방법
```sh
./compatible_function_pointer
```
## 예상 관찰 결과
선택한 compatible function의 계산 결과가 출력된다.
## 확인 포인트
incompatible cast, null call, uninitialized call, function pointer arithmetic을 작성하지 않는다.
## 추가 실습
- ★ 현재 target을 equality로 확인한다.
- ★★ null 상태에서는 메시지만 출력한다.
- ★★★ incompatible 선언의 문제를 실행 없이 분석한다.
## 완료 기준
compatible target만 사용하고 null check 뒤 warning 없이 실행한다.
