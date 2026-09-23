# 23-14 실습: Part 23 종합 복습
이론: [note](../../../notes/23-file-io/23-14-part-23-review.md)
## 실습 목적
student record save/load lifecycle과 모든 result checks를 종합한다.
## 작성할 파일
`part23_review.c`
## 해야 할 일
한 record를 explicit text format으로 저장하고 bounded conversion으로 복원한다.
## 사용할 개념
stream lifecycle, format contract, assignment count, ranges, cleanup.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic part23_review.c -o part23_review
```
## 실행 방법
```sh
./part23_review
```
## 예상 관찰 결과
`1001 88.50 Park`가 출력된다.
## 확인 포인트
FILE/fd, EOF, text/binary, raw struct portability를 정확히 구분한다.
## 추가 실습
- ★ 여러 records를 처리한다.
- ★★ malformed record 위치를 보고한다.
- ★★★ versioned format을 설계한다.
## 완료 기준
모든 open·I/O·close results를 검사하고 warning 없이 round trip한다.
