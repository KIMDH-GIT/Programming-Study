# 12-7 실습: 전치 행렬

이론: [note](../../../notes/12-two-dimensional-arrays/12-7-transpose.md)
## 실습 목적
row와 column을 바꾸어 별도 transpose array를 만든다.
## 작성할 파일
`matrix_transpose.c`
## 해야 할 일
`{{1,2,3},{4,5,6}}`을 3x2 array로 transpose해 출력한다.
## 사용할 개념
transpose shape, swapped indices, nested loop, separate result array.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic matrix_transpose.c -o matrix_transpose
```
## 실행 방법
```sh
./matrix_transpose
```
## 예상 관찰 결과
`1 4`, `2 5`, `3 6`이 세 rows로 출력된다.
## 확인 포인트
결과 shape와 destination `[column][row]`가 정확한가?
## 추가 실습
- ★ 2x2 - ★★ 3x2 - ★★★ index 대응표
## 완료 기준
- [ ] 경고 없음 - [ ] 3x2 결과 정확 - [ ] 별도 배열 생성
