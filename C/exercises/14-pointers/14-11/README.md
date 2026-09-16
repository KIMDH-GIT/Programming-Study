# 14-11 실습: 잘못된 포인터 접근

이론: [note](../../../notes/14-pointers/14-11-invalid-pointer-access.md)
## 실습 목적
pointer state를 검사하고 valid target에서만 dereference한다.
## 작성할 파일
`safe_pointer_access.c`
## 해야 할 일
NULL pointer에 valid object address를 저장한 뒤 non-null branch에서 값을 읽는다.
## 사용할 개념
NULL state, valid target, guarded dereference, UB classification.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic safe_pointer_access.c -o safe_pointer_access
```
## 실행 방법
```sh
./safe_pointer_access
```
## 예상 관찰 결과
valid object value만 출력된다.
## 확인 포인트
invalid examples를 실제로 dereference하지 않는가?
## 추가 실습
- ★ branch - ★★ state table - ★★★ invalid case analysis
## 완료 기준
- [ ] 경고 없음 - [ ] guarded dereference - [ ] UB 사례 실행 안 함
