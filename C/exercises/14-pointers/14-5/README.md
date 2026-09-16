# 14-5 실습: 포인터를 통한 값 수정

이론: [note](../../../notes/14-pointers/14-5-modify-through-pointer.md)
## 실습 목적
indirection assignment가 pointed-to object를 수정함을 확인한다.
## 작성할 파일
`modify_through_pointer.c`
## 해야 할 일
int object를 pointer로 20, 30으로 바꾸고 각 단계에서 object value를 출력한다.
## 사용할 개념
modifiable lvalue, indirection, assignment, pointed-to object.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic modify_through_pointer.c -o modify_through_pointer
```
## 실행 방법
```sh
./modify_through_pointer
```
## 예상 관찰 결과
초기값과 수정된 20, 30이 순서대로 확인된다.
## 확인 포인트
pointer value와 pointed-to value 변경을 구분하는가?
## 추가 실습
- ★ double 수정 - ★★ aliases - ★★★ 변화표
## 완료 기준
- [ ] 경고 없음 - [ ] 수정값 정확 - [ ] object 구분
