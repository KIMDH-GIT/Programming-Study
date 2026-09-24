# 24-11 실습: 학생 관리 프로그램 파일 분리
이론: [note](../../../notes/24-multi-file-programs/24-11-splitting-student-management-program.md)
## 실습 목적
학생 type, public API, private helper를 세 파일로 분리한다.
## 작성할 파일
- `main.c`
- `student.c`
- `student.h`
## 해야 할 일
학생 pass 판정과 출력을 public functions로 구현하고 결과 문자열 helper는 source 내부 `static`으로 둔다.
## 사용할 개념
shared struct, public interface, file-local helper, pointer parameter.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c student.c -o student_app
```
## 실행 방법
```sh
./student_app
```
## 예상 관찰 결과
`1001 Kim 87.5 PASS`가 출력된다.
## 확인 포인트
공유 type을 복붙하지 않고 public declarations와 private helper를 분리한다.
## 추가 실습
- ★ 학생 배열을 출력한다.
- ★★ 평균 계산 public function을 추가한다.
- ★★★ public name prefix의 장점을 설명한다.
## 완료 기준
세 파일의 책임이 명확하고 strict C17 build·실행이 성공한다.
