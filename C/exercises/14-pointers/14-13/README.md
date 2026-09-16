# 14-13 실습: Part 14 종합 복습

이론: [note](../../../notes/14-pointers/14-13-part-14-review.md)
## 실습 목적
valid pointer read·modify·NULL 전환을 종합한다.
## 작성할 파일
`part14_review.c`
## 해야 할 일
int object를 pointer로 읽고 수정한 뒤 pointer를 NULL로 설정해 검사한다.
## 사용할 개념
pointer declaration, `&`, `*`, modification, NULL, valid lifetime.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic part14_review.c -o part14_review
```
## 실행 방법
```sh
./part14_review
```
## 예상 관찰 결과
pointer 표현, 수정 전후 values, `no target`이 출력된다.
## 확인 포인트
NULL 설정 뒤 dereference하지 않고 array/pointer arithmetic을 사용하지 않는가?
## 추가 실습
- ★ type 표 - ★★ alias - ★★★ 계층/상태 점검표
## 완료 기준
- [ ] 경고 없음 - [ ] 수정 결과 정확 - [ ] Part 15 파일 미생성
## 다음 Step
Step 15-1. 배열 이름과 첫 원소 주소
