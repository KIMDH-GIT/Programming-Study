# 19-16 실습: Part 19 종합 복습
이론: [note](../../../notes/19-structures/19-16-part-19-review.md)
## 실습 목적
Part 19의 structure와 학생 관리 규칙을 종합한다.
## 작성할 파일
`part19_review.c`
## 해야 할 일
Student array에 등록·검색·수정·삭제·정렬을 적용하고 모든 결과를 출력한다.
## 사용할 개념
tag/typedef, `.`, `->`, functions, validated input, invariant.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic part19_review.c -lm -o part19_review
```
## 실행 방법
```sh
./part19_review
```
## 예상 관찰 결과
각 operation 뒤 count와 records가 예상 상태를 유지한다.
## 확인 포인트
structure equality, raw-byte comparison, automatic deep copy에 의존하지 않는다.
## 추가 실습
- ★ 용어 표를 만든다.
- ★★ 함수 contract를 검토한다.
- ★★★ shallow-copy 위험을 코드 실행 없이 분석한다.
## 완료 기준
Part 19 문법과 invariant를 설명하고 warning 없이 실행한다.
