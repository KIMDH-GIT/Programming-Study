# 14-10 실습: 포인터 타입과 접근형

이론: [note](../../../notes/14-pointers/14-10-pointer-type-access.md)
## 실습 목적
pointer size와 pointed-to object size를 구분한다.
## 작성할 파일
`pointer_types.c`
## 해야 할 일
int, double, char pointers와 pointed objects의 `sizeof`를 `%zu`로 출력한다.
## 사용할 개념
pointer type, access type, `sizeof(pointer)`, `sizeof(*pointer)`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic pointer_types.c -o pointer_types
```
## 실행 방법
```sh
./pointer_types
```
## 예상 관찰 결과
현재 implementation의 pointer/object sizes가 구분되어 출력된다.
## 확인 포인트
관찰값을 모든 C implementation의 보장으로 일반화하지 않는가?
## 추가 실습
- ★ char size - ★★ implementation 표 - ★★★ mismatch 분석
## 완료 기준
- [ ] 경고 없음 - [ ] `%zu` 사용 - [ ] type/size 구분
