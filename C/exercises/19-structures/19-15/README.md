# 19-15 실습: 학생 관리 프로그램 통합
이론: [note](../../../notes/19-structures/19-15-student-management-integration.md)
## 실습 목적
Part 19 기능을 하나의 bounded CLI로 통합한다.
## 작성할 파일
`student_manager.c`
## 해야 할 일
add/delete/search/list/sort/quit 메뉴와 검증된 정수·실수 입력을 구현한다.
## 사용할 개념
Student array, count/capacity, `strtol`, `strtod`, functions.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic student_manager.c -lm -o student_manager
```
## 실행 방법
```sh
./student_manager
```
## 예상 관찰 결과
각 명령이 invariant를 보존하며 반복 실행된다.
## 확인 포인트
중복·없는 id·full·잘못된 입력 경로를 처리한다.
## 추가 실습
- ★ 메시지를 정리한다.
- ★★ 시나리오를 기록한다.
- ★★★ 함수 contract를 작성한다.
## 완료 기준
정상·오류·종료 경로가 warning 없이 동작한다.
