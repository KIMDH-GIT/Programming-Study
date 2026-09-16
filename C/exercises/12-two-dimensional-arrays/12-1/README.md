# 12-1 실습: 행·열과 2차원 배열 선언

이론: [note](../../../notes/12-two-dimensional-arrays/12-1-rows-columns-declaration.md)
## 실습 목적
array-of-arrays 구조와 row·column index를 구분한다.
## 작성할 파일
`matrix_declaration.c`
## 해야 할 일
2x3 `int` 배열을 초기화하고 네 모서리 element를 출력한다.
## 사용할 개념
row, column, array of arrays, two subscripts, valid index.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic matrix_declaration.c -o matrix_declaration
```
## 실행 방법
```sh
./matrix_declaration
```
## 예상 관찰 결과
첫·마지막 row와 column의 네 값이 초기화 순서대로 출력된다.
## 확인 포인트
row 범위 0~1과 column 범위 0~2를 지켰는가?
## 추가 실습
- ★ 3x2 선언 - ★★ index 해석 - ★★★ 유효성 판별
## 완료 기준
- [ ] 경고 없이 컴파일 - [ ] 네 값 정확 - [ ] array-of-arrays 설명
