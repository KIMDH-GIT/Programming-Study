# 16-9 실습: Part 16 종합 복습

이론: [note](../../../notes/16-pointers-and-functions/16-9-part-16-review.md)
## 실습 목적
pointer로 scalar와 array elements를 수정하며 pass-by-value를 종합한다.
## 작성할 파일
`part16_review.c`
## 해야 할 일
pointer scalar update와 counted array update를 호출하고 결과를 출력한다.
## 사용할 개념
pass-by-value, pointer parameter, array adjustment, `size_t` count.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic part16_review.c -o part16_review
```
## 실행 방법
```sh
./part16_review
```
## 예상 관찰 결과
scalar 50과 array 2,3,4가 출력된다.
## 확인 포인트
두 calls를 call by reference가 아니라 pointer value copies로 설명하는가?
## 추가 실습
- ★ parameter table - ★★ swap/update - ★★★ call diagram
## 완료 기준
- [ ] 경고 없음 - [ ] 50/2/3/4 정확 - [ ] Part 17 파일 미생성
## 다음 Step
Step 17-1. `const` 객체
