# 22-6 실습: 함수 포인터 배열
이론: [note](../../../notes/22-function-pointers/22-6-function-pointer-array.md)
## 실습 목적
array of function pointers를 선언하고 bounds 검사 뒤 dispatch한다.
## 작성할 파일
`function_pointer_array.c`
## 해야 할 일
세 arithmetic function pointers를 array에 넣고 valid index의 target을 호출한다.
## 사용할 개념
array declarator, function pointer element, `sizeof` element count, bounds check.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic function_pointer_array.c -o function_pointer_array
```
## 실행 방법
```sh
./function_pointer_array
```
## 예상 관찰 결과
선택 index에 해당하는 operation 결과가 출력된다.
## 확인 포인트
`index < count`를 검사하고 pointer-to-array와 선언을 구분한다.
## 추가 실습
- ★ 모든 valid indices를 순회한다.
- ★★ invalid index에 오류 메시지를 출력한다.
- ★★★ 두 array 관련 declarators를 비교한다.
## 완료 기준
out-of-bounds access 없이 warning 없이 dispatch한다.
