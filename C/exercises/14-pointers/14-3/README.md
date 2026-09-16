# 14-3 실습: 포인터 변수 선언

이론: [note](../../../notes/14-pointers/14-3-pointer-declaration.md)
## 실습 목적
pointed-to type에 맞는 pointer declarations를 읽고 쓴다.
## 작성할 파일
`pointer_declarations.c`
## 해야 할 일
int, double, char objects와 각각 맞는 pointer objects를 선언한다.
## 사용할 개념
pointer type, declarator `*`, pointed-to type, one declaration per identifier.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic pointer_declarations.c -o pointer_declarations
```
## 실행 방법
```sh
./pointer_declarations
```
## 예상 관찰 결과
세 pointer values가 `%p`로 출력된다.
## 확인 포인트
`int *p, q` 함정을 피하고 types를 정확히 읽는가?
## 추가 실습
- ★ mixed declaration 분석 - ★★ `sizeof` 비교 - ★★★ 보장/관찰 표
## 완료 기준
- [ ] 경고 없음 - [ ] types 정확 - [ ] 8-byte 단정 없음
