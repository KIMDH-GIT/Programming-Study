# 15-2 실습: 배열 객체와 포인터 변수의 차이

이론: [note](../../../notes/15-arrays-and-pointers/15-2-array-object-pointer-variable.md)
## 실습 목적
array·pointer·element 크기와 address type을 구분한다.
## 작성할 파일
`array_pointer_difference.c`
## 해야 할 일
`sizeof(array)`, `sizeof(pointer)`, `sizeof(*pointer)`와 세 address forms를 출력한다.
## 사용할 개념
conversion exception, `sizeof`, pointer-to-array, distinct types.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic array_pointer_difference.c -o array_pointer_difference
```
## 실행 방법
```sh
./array_pointer_difference
```
## 예상 관찰 결과
현재 구현의 서로 다른 크기 의미와 같은 시작 address 표현이 관찰된다.
## 확인 포인트
관찰값과 type semantics를 분리하는가?
## 추가 실습
- ★ char array - ★★ type 읽기 - ★★★ context 표
## 완료 기준
- [ ] 경고 없음 - [ ] 세 sizeof 구분 - [ ] array != pointer
