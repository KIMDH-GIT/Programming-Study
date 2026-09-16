# 12-5 실습: `sizeof`로 행·원소 크기 확인

이론: [note](../../../notes/12-two-dimensional-arrays/12-5-sizeof-rows-elements.md)
## 실습 목적
전체 matrix·row·element 크기와 counts를 구분한다.
## 작성할 파일
`matrix_sizeof.c`
## 해야 할 일
2x3 `int` 배열의 세 `sizeof`와 rows·columns를 `%zu`로 출력한다.
## 사용할 개념
`sizeof`, `size_t`, `%zu`, row array, scalar element.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic matrix_sizeof.c -o matrix_sizeof
```
## 실행 방법
```sh
./matrix_sizeof
```
## 예상 관찰 결과
rows 2, columns 3이 출력되고 byte 수는 구현의 `sizeof(int)`에 따른다.
## 확인 포인트
byte 수와 element count를 구분했는가?
## 추가 실습
- ★ 3x2 - ★★ `double` 2x2 - ★★★ 크기 관계식
## 완료 기준
- [ ] 경고 없음 - [ ] rows 2 columns 3 - [ ] `%zu` 사용
