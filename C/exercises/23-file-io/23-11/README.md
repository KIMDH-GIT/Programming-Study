# 23-11 실습: 학생 데이터 저장
이론: [note](../../../notes/23-file-io/23-11-save-student-data.md)
## 실습 목적
student array를 명시한 format으로 안전하게 저장한다.
## 작성할 파일
`save_students.c`
## 해야 할 일
두 records를 학습용 file에 쓰고 각 write와 close result를 검사한다.
## 사용할 개념
structure array, `fprintf`, count, cleanup, truncation.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic save_students.c -o save_students
```
## 실행 방법
```sh
./save_students
```
## 예상 관찰 결과
`saved: 2`가 출력된다.
## 확인 포인트
실제 사용자 file을 overwrite하지 않고 failure 전에 success를 출력하지 않는다.
## 추가 실습
- ★ record를 하나 추가한다.
- ★★ ranges를 save 전에 검사한다.
- ★★★ cleanup flow를 설계한다.
## 완료 기준
모든 records와 close 결과를 검사해 warning 없이 저장한다.
