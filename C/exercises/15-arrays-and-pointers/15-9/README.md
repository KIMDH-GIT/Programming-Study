# 15-9 실습: 시작 전 포인터를 만들지 않는 역순 순회

이론: [note](../../../notes/15-arrays-and-pointers/15-9-safe-reverse-traversal.md)
## 실습 목적
before-begin pointer 없이 reverse traversal한다.
## 작성할 파일
`safe_reverse_pointer.c`
## 해야 할 일
one-past에서 시작해 먼저 감소하고 다섯 values를 역순 출력한다.
## 사용할 개념
one-past, decrement-before-dereference, begin boundary, reverse loop.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic safe_reverse_pointer.c -o safe_reverse_pointer
```
## 실행 방법
```sh
./safe_reverse_pointer
```
## 예상 관찰 결과
50, 40, 30, 20, 10이 출력된다.
## 확인 포인트
values-1을 만들거나 one-past를 dereference하지 않는가?
## 추가 실습
- ★ count 1 - ★★ reverse sum - ★★★ state table
## 완료 기준
- [ ] 경고 없음 - [ ] 역순 정확 - [ ] boundaries 안전
