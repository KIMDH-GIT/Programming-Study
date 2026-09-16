# 16-2 실습: 포인터 매개변수로 주소 전달

이론: [note](../../../notes/16-pointers-and-functions/16-2-pointer-parameter-address.md)
## 실습 목적
caller pointer와 pointer parameter object를 구분한다.
## 작성할 파일
`pointer_parameter.c`
## 해야 할 일
pointer parameter의 value와 address를 출력하고 NULL 재지정 후 caller pointer를 확인한다.
## 사용할 개념
pointer argument, parameter object, pass-by-value, parameter reassignment.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic pointer_parameter.c -o pointer_parameter
```
## 실행 방법
```sh
./pointer_parameter
```
## 예상 관찰 결과
callee 재지정 후에도 caller pointer가 valid value를 읽는다.
## 확인 포인트
pointer value와 pointer parameter object address를 구분하는가?
## 추가 실습
- ★ retarget - ★★ state table - ★★★ ABI distinction
## 완료 기준
- [ ] 경고 없음 - [ ] caller pointer 유지 - [ ] pass-by-value 설명
