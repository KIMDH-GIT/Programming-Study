# 15-11 실습: Part 15 종합 복습

이론: [note](../../../notes/15-arrays-and-pointers/15-11-part-15-review.md)
## 실습 목적
array/pointer sizes, distance, traversal과 bounds를 종합한다.
## 작성할 파일
`part15_review.c`
## 해야 할 일
actual array와 pointer `sizeof`, begin/end distance, pointer traversal values를 출력한다.
## 사용할 개념
array conversion, `sizeof`, one-past, `ptrdiff_t`, pointer loop.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic part15_review.c -o part15_review
```
## 실행 방법
```sh
./part15_review
```
## 예상 관찰 결과
implementation sizes, distance 5, five values가 출력된다.
## 확인 포인트
array!=pointer, one-past non-dereference, pass-by-value 원칙을 설명하는가?
## 추가 실습
- ★ expression 비교 - ★★ reverse - ★★★ rules checklist
## 완료 기준
- [ ] 경고 없음 - [ ] distance 5 - [ ] Part 16 파일 미생성
## 다음 Step
Step 16-1. 값 매개변수로 원본을 바꾸지 못하는 이유
