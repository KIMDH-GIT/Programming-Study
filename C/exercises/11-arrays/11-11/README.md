# 11-11 실습: Part 11 종합 복습

이론: [note](../../../notes/11-arrays/11-11-part-11-review.md)
## 실습 목적
배열 초기화·count·순회·합·경계를 한 프로그램에서 종합한다.
## 작성할 파일
`part11_review.c`
## 해야 할 일
`{3, 1, 4, 1, 5}`의 count, 모든 element, 합을 출력한다.
## 사용할 개념
array initialization, `sizeof`, `size_t`, traversal, accumulation, bounds.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic part11_review.c -o part11_review
```
## 실행 방법
```sh
./part11_review
```
## 예상 관찰 결과
count 5, 다섯 값, 합 14가 출력된다.
## 확인 포인트
모든 subscript가 count 미만이고 byte 수와 count를 구분하는가?
## 추가 실습
- ★ 0 초기화 - ★★ 최댓값과 검색 - ★★★ 오류 코드 분류
## 완료 기준
- [ ] 경고 없음 - [ ] count 5와 합 14 - [ ] Part 12 파일 미생성
## 다음 Step
Step 12-1. 행·열과 2차원 배열 선언
