# 19-13 실습: 학생 목록
이론: [note](../../../notes/19-structures/19-13-student-list.md)
## 실습 목적
유효 학생 records만 출력한다.
## 작성할 파일
`student_list.c`
## 해야 할 일
빈 목록과 비어 있지 않은 목록을 const 함수로 출력한다.
## 사용할 개념
const array parameter, count, format specifier.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic student_list.c -o student_list
```
## 실행 방법
```sh
./student_list
```
## 예상 관찰 결과
빈 경우 안내, 그 외에는 모든 record가 출력된다.
## 확인 포인트
capacity가 아니라 count를 사용한다.
## 추가 실습
- ★ 표 머리글을 추가한다.
- ★★ 평균을 출력한다.
- ★★★ 출력 형식을 함수로 분리한다.
## 완료 기준
빈/여러 record 경로가 warning 없이 동작한다.
