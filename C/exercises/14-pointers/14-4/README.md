# 14-4 실습: 역참조 연산자 `*`

이론: [note](../../../notes/14-pointers/14-4-indirection-operator.md)
## 실습 목적
valid pointer를 dereference해 pointed-to value를 읽는다.
## 작성할 파일
`pointer_indirection.c`
## 해야 할 일
int object와 `*pointer` 값을 각각 출력해 같은 object 접근임을 확인한다.
## 사용할 개념
pointer declaration, indirection, lvalue, pointed-to object.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic pointer_indirection.c -o pointer_indirection
```
## 실행 방법
```sh
./pointer_indirection
```
## 예상 관찰 결과
object와 dereference 값이 동일하게 출력된다.
## 확인 포인트
pointer를 먼저 valid object address로 초기화했는가?
## 추가 실습
- ★ double read - ★★ aliases - ★★★ star 역할 분류
## 완료 기준
- [ ] 경고 없음 - [ ] 값 동일 - [ ] dereference 전제 설명
