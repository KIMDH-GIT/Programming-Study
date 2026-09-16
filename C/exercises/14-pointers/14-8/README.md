# 14-8 실습: `NULL` pointer

이론: [note](../../../notes/14-pointers/14-8-null-pointer.md)
## 실습 목적
null 상태를 검사하고 valid 상태에서만 dereference한다.
## 작성할 파일
`null_pointer.c`
## 해야 할 일
pointer를 NULL로 초기화하고 검사한 뒤 int object address를 저장해 값을 출력한다.
## 사용할 개념
NULL, null pointer value, comparison, valid dereference, UB avoidance.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic null_pointer.c -o null_pointer
```
## 실행 방법
```sh
./null_pointer
```
## 예상 관찰 결과
null 상태 문장과 valid object value가 출력된다.
## 확인 포인트
NULL 상태에서는 `*pointer`를 평가하지 않는가?
## 추가 실습
- ★ branches - ★★ reset to NULL - ★★★ 개념 표
## 완료 기준
- [ ] 경고 없음 - [ ] null 검사 - [ ] null dereference 없음
