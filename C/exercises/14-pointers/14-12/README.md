# 14-12 실습: 포인터 기초 종합 실습

이론: [note](../../../notes/14-pointers/14-12-pointer-basics-practice.md)
## 실습 목적
두 aliases로 같은 object를 읽고 수정한다.
## 작성할 파일
`pointer_basics_practice.c`
## 해야 할 일
two int pointers가 같은 object를 가리키게 하고 한 pointer로 수정한 뒤 다른 pointer로 읽는다.
## 사용할 개념
address-of, pointer object, indirection, alias, pointed-to object.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic pointer_basics_practice.c -o pointer_basics_practice
```
## 실행 방법
```sh
./pointer_basics_practice
```
## 예상 관찰 결과
두 pointers가 초기값과 수정된 동일한 value를 관찰한다.
## 확인 포인트
aliases와 pointer objects 자체를 구분하는가?
## 추가 실습
- ★ three aliases - ★★ retarget - ★★★ state diagram
## 완료 기준
- [ ] 경고 없음 - [ ] alias 결과 정확 - [ ] ownership 오해 없음
