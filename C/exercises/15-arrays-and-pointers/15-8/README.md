# 15-8 실습: 포인터로 배열 순회

이론: [note](../../../notes/15-arrays-and-pointers/15-8-pointer-traversal.md)
## 실습 목적
begin/end pointers로 array를 forward traversal한다.
## 작성할 파일
`pointer_traversal.c`
## 해야 할 일
pointer-only loop로 다섯 values를 출력한다.
## 사용할 개념
current pointer, one-past end, increment, indirection, comparison.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic pointer_traversal.c -o pointer_traversal
```
## 실행 방법
```sh
./pointer_traversal
```
## 예상 관찰 결과
10부터 50까지 순서대로 출력된다.
## 확인 포인트
end를 dereference하지 않고 compact precedence 표현을 피하는가?
## 추가 실습
- ★ sum - ★★ modify - ★★★ index 비교
## 완료 기준
- [ ] 경고 없음 - [ ] 다섯 값 정확 - [ ] endpoint 안전
