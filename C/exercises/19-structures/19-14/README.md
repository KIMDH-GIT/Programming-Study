# 19-14 실습: 학생 정렬
이론: [note](../../../notes/19-structures/19-14-student-sort.md)
## 실습 목적
record integrity를 보존하며 정렬한다.
## 작성할 파일
`student_sort.c`
## 해야 할 일
Student 전체를 교환해 id 오름차순으로 정렬한다.
## 사용할 개념
nested loop, member comparison, structure assignment.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic student_sort.c -o student_sort
```
## 실행 방법
```sh
./student_sort
```
## 예상 관찰 결과
id가 오름차순이고 각 score는 원래 id와 함께 이동한다.
## 확인 포인트
key member만 교환하지 않는다.
## 추가 실습
- ★ score 정렬을 한다.
- ★★ tie-break를 추가한다.
- ★★★ 정렬 전후 record 집합을 확인한다.
## 완료 기준
bounds 안에서 record integrity를 유지한다.
