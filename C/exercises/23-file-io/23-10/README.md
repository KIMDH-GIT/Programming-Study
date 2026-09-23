# 23-10 실습: 학생 레코드 파일 형식 설계
이론: [note](../../../notes/23-file-io/23-10-student-record-format-design.md)
## 실습 목적
student text record의 field order와 validation contract를 정의한다.
## 작성할 파일
`student_record_format.c`
## 해야 할 일
id·score·한 단어 name을 문서화한 순서로 저장한다.
## 사용할 개념
text format, delimiter, range, maximum length, versioning.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic student_record_format.c -o student_record_format
```
## 실행 방법
```sh
./student_record_format
```
## 예상 관찰 결과
sample student record가 출력되고 file에 같은 fields가 저장된다.
## 확인 포인트
단순 whitespace format을 완전한 CSV라고 부르지 않는다.
## 추가 실습
- ★ invalid records를 적는다.
- ★★ version field 필요성을 분석한다.
- ★★★ whitespace name encoding을 설계한다.
## 완료 기준
format contract와 지원하지 않는 입력을 명확히 설명한다.
