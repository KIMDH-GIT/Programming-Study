# 15-5 실습: pointer arithmetic의 원소 단위

이론: [note](../../../notes/15-arrays-and-pointers/15-5-pointer-arithmetic-element-unit.md)
## 실습 목적
pointer +1을 pointed-to type의 next element로 해석한다.
## 작성할 파일
`pointer_element_unit.c`
## 해야 할 일
int와 double arrays에서 first와 second elements를 pointer expressions로 출력한다.
## 사용할 개념
pointer addition, pointed-to type, next element, implementation size.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic pointer_element_unit.c -o pointer_element_unit
```
## 실행 방법
```sh
./pointer_element_unit
```
## 예상 관찰 결과
각 array의 first·second values와 element sizes가 출력된다.
## 확인 포인트
pointer arithmetic을 고정 byte 수 규칙으로 설명하지 않는가?
## 추가 실습
- ★ char array - ★★ third element - ★★★ semantic/address 비교
## 완료 기준
- [ ] 경고 없음 - [ ] next values 정확 - [ ] element 단위 설명
