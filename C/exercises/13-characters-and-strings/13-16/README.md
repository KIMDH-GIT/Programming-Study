# 13-16 실습: Part 13 종합 복습

이론: [note](../../../notes/13-characters-and-strings/13-16-part-13-review.md)
## 실습 목적
valid string의 출력·length·array size·digit conversion을 종합한다.
## 작성할 파일
`part13_review.c`
## 해야 할 일
`"C17"` array의 내용, `strlen`, `sizeof`, 마지막 digit value를 출력한다.
## 사용할 개념
char array, null terminator, `%s`, `strlen`, `sizeof`, digit continuity.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic part13_review.c -o part13_review
```
## 실행 방법
```sh
./part13_review
```
## 예상 관찰 결과
`C17`, length 3, array bytes 4, digit value 7이 출력된다.
## 확인 포인트
length와 capacity를 구분하고 ASCII code값을 가정하지 않는가?
## 추가 실습
- ★ 직접 length - ★★ copy와 compare - ★★★ 규칙 점검표
## 완료 기준
- [ ] 경고 없음 - [ ] 3/4/7 정확 - [ ] Part 14 파일 미생성
## 다음 Step
Step 14-1. 메모리 주소란 무엇인가
