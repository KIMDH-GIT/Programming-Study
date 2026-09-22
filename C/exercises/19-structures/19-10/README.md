# 19-10 실습: 학생 등록과 중복 검사
이론: [note](../../../notes/19-structures/19-10-student-registration-duplicate-check.md)
## 실습 목적
bounded array에 unique record를 추가한다.
## 작성할 파일
`student_add.c`
## 해야 할 일
중복 id와 full 상태를 거부하고 성공 때만 count를 늘린다.
## 사용할 개념
structure array, search, capacity, pointer output.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic student_add.c -o student_add
```
## 실행 방법
```sh
./student_add
```
## 예상 관찰 결과
새 id는 등록되고 중복 id는 거부된다.
## 확인 포인트
실패 시 배열과 count가 보존된다.
## 추가 실습
- ★ full case를 시험한다.
- ★★ 이름을 추가한다.
- ★★★ 결과 enum 없이 상수 code를 문서화한다.
## 완료 기준
중복·용량·성공 세 경로가 정확하다.
