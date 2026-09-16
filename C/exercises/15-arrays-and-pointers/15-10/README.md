# 15-10 실습: 포인터만 이용한 배열 실습

이론: [note](../../../notes/15-arrays-and-pointers/15-10-pointer-only-array-practice.md)
## 실습 목적
subscript 없이 sum과 maximum을 계산한다.
## 작성할 파일
`pointer_only_array.c`
## 해야 할 일
begin/end pointers와 indirection으로 five values의 sum·maximum을 출력한다.
## 사용할 개념
pointer traversal, one-past, indirection, accumulation, nonempty array.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic pointer_only_array.c -o pointer_only_array
```
## 실행 방법
```sh
./pointer_only_array
```
## 예상 관찰 결과
예제 array의 sum 13과 max 7이 출력된다.
## 확인 포인트
processing loop에 subscript가 없고 end를 dereference하지 않는가?
## 추가 실습
- ★ minimum - ★★ all-negative - ★★★ first match
## 완료 기준
- [ ] 경고 없음 - [ ] 13과 7 - [ ] pointer bounds 안전
