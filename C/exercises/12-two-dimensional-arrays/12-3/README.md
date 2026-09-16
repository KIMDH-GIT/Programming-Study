# 12-3 실습: 중첩 반복문 순회

이론: [note](../../../notes/12-two-dimensional-arrays/12-3-nested-loop-traversal.md)
## 실습 목적
row와 column loop로 모든 elements를 순회한다.
## 작성할 파일
`matrix_traversal.c`
## 해야 할 일
2x3 배열을 nested loop로 표처럼 출력한다.
## 사용할 개념
outer row loop, inner column loop, bounds, access count, newline.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic matrix_traversal.c -o matrix_traversal
```
## 실행 방법
```sh
./matrix_traversal
```
## 예상 관찰 결과
두 rows가 각각 한 줄에 세 values로 출력된다.
## 확인 포인트
여섯 elements를 정확히 한 번씩 접근하고 newline이 row 끝에 있는가?
## 추가 실습
- ★ 3x2 출력 - ★★ 전체 합 - ★★★ 추적표
## 완료 기준
- [ ] 경고 없음 - [ ] 2x3 표 출력 - [ ] 두 축 경계 정확
