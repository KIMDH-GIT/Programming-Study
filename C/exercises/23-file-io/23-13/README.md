# 23-13 실습: 재실행 후 데이터 유지 확인
이론: [note](../../../notes/23-file-io/23-13-persistence-across-runs.md)
## 실습 목적
별도 saver·loader processes 사이 student data persistence를 확인한다.
## 작성할 파일
`verify_students.c`
## 해야 할 일
23-11의 `save_students.c`를 실행한 뒤 같은 file을 read-only로 여는 verifier를 실행한다.
## 사용할 개념
persistence, shared format contract, read-only verification, process lifetime.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic verify_students.c -o verify_students
```
## 실행 방법
```sh
./save_students
./verify_students
```
## 예상 관찰 결과
saver가 종료된 뒤 verifier가 `restored: 1001 88.50 Park`를 출력한다.
## 확인 포인트
두 programs가 같은 file name과 format을 쓰며 verifier가 file을 변경하지 않는지 확인한다.
## 추가 실습
- ★ student value를 바꿔 저장한다.
- ★★ malformed student file을 거부한다.
- ★★★ format version migration을 설계한다.
## 완료 기준
임시 directory에서 두 processes의 save/load 결과를 확인하고 artifacts를 제거한다.
