# 19-8 실습: `strtol`로 정수 입력 검증
이론: [note](../../../notes/19-structures/19-8-strtol-integer-input-validation.md)
## 실습 목적
문자열 입력을 검증된 `int`로 변환한다.
## 작성할 파일
`parse_student_id.c`
## 해야 할 일
`fgets`의 EOF·오류·truncated line을 처리한 뒤 `strtol`로 parse해 양의 학번만 출력한다.
## 사용할 개념
`errno`, `ERANGE`, end pointer, `INT_MIN`, `INT_MAX`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic parse_student_id.c -o parse_student_id
```
## 실행 방법
```sh
./parse_student_id
```
## 예상 관찰 결과
유효 정수는 출력되고 문자 혼합·범위 초과·buffer보다 긴 입력은 거부된다.
## 확인 포인트
0 반환만으로 성공 여부를 판단하지 않고, newline 제거 뒤 `*end == '\0'`를 확인한다.
## 추가 실습
- ★ 범위를 제한한다.
- ★★ 공백 policy를 구현한다.
- ★★★ 유효할 때까지 다시 묻는다.
## 완료 기준
syntax, trailing 문자, range 오류를 모두 구분한다.
