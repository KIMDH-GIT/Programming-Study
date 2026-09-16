# 15-3 실습: `arr[i]`와 `*(arr + i)`

이론: [note](../../../notes/15-arrays-and-pointers/15-3-subscript-indirection.md)
## 실습 목적
subscript를 conversion·addition·indirection으로 해석한다.
## 작성할 파일
`subscript_indirection.c`
## 해야 할 일
모든 elements를 `arr[i]`, `*(arr+i)`, `pointer[i]`로 출력한다.
## 사용할 개념
array-to-pointer conversion, pointer addition, indirection, subscript.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic subscript_indirection.c -o subscript_indirection
```
## 실행 방법
```sh
./subscript_indirection
```
## 예상 관찰 결과
각 index의 세 값이 동일하다.
## 확인 포인트
모든 indices가 bounds 안이고 array!=pointer 원칙을 유지하는가?
## 추가 실습
- ★ pointer subscript 수정 - ★★ double array - ★★★ 단계도
## 완료 기준
- [ ] 경고 없음 - [ ] 값 일치 - [ ] 정의 설명
