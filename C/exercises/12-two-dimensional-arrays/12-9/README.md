# 12-9 실습: Part 12 종합 복습

이론: [note](../../../notes/12-two-dimensional-arrays/12-9-part-12-review.md)
## 실습 목적
shape, `sizeof`, nested traversal, 표 출력과 전체 합을 종합한다.
## 작성할 파일
`part12_review.c`
## 해야 할 일
2x3 matrix의 rows·columns를 계산하고 모든 values와 합을 출력한다.
## 사용할 개념
array of arrays, `sizeof`, `size_t`, nested loop, row-major, bounds.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic part12_review.c -o part12_review
```
## 실행 방법
```sh
./part12_review
```
## 예상 관찰 결과
2x3 표, rows 2, columns 3, 합 21이 확인된다.
## 확인 포인트
두 축 모두 `<` 경계를 사용하고 여섯 elements를 한 번씩 처리하는가?
## 추가 실습
- ★ row 합 - ★★ column 합 - ★★★ target 위치 검색
## 완료 기준
- [ ] 경고 없음 - [ ] rows 2 columns 3 sum 21 - [ ] Part 13 파일 미생성
## 다음 Step
Step 13-1. `char`와 문자 `'A'`
