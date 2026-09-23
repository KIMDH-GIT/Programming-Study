# 23-12 실습: 학생 데이터 복원
이론: [note](../../../notes/23-file-io/23-12-load-student-data.md)
## 실습 목적
bounded array에 valid student records만 복원한다.
## 작성할 파일
`load_students.c`
## 해야 할 일
두 records를 읽어 assignment count·capacity·ranges를 검사한다.
## 사용할 개념
`fscanf`, capacity, malformed input, EOF, range validation.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic load_students.c -o load_students
```
## 실행 방법
```sh
./load_students
```
## 예상 관찰 결과
`loaded: 2`가 출력된다.
## 확인 포인트
partial assignment와 malformed data를 valid record로 세지 않는다.
## 추가 실습
- ★ 한 record만 읽는다.
- ★★ invalid score를 거부한다.
- ★★★ line-based parser를 설계한다.
## 완료 기준
capacity와 record validation을 지키며 warning 없이 복원한다.
