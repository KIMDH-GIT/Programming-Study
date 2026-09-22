# 19-11 실습: 학생 삭제
이론: [note](../../../notes/19-structures/19-11-student-deletion.md)
## 실습 목적
삭제 뒤 array invariant를 유지한다.
## 작성할 파일
`student_delete.c`
## 해야 할 일
id를 찾아 뒤 records를 당기고 성공 때만 count를 줄인다.
## 사용할 개념
linear search, structure assignment, bounds.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic student_delete.c -o student_delete
```
## 실행 방법
```sh
./student_delete
```
## 예상 관찰 결과
대상만 사라지고 상대 순서가 유지된다.
## 확인 포인트
없는 id와 마지막 id에서도 bounds가 안전하다.
## 추가 실습
- ★ 네 경계를 시험한다.
- ★★ 이동 횟수를 출력한다.
- ★★★ 비순서 보존 삭제를 비교한다.
## 완료 기준
성공·실패 모두 count invariant를 지킨다.
