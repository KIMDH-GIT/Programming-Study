# 12-6 실습: 행렬 덧셈

이론: [note](../../../notes/12-two-dimensional-arrays/12-6-matrix-addition.md)
## 실습 목적
같은 위치의 elements를 더해 결과 matrix를 만든다.
## 작성할 파일
`matrix_addition.c`
## 해야 할 일
두 2x3 matrix를 더해 결과를 표 형태로 출력한다.
## 사용할 개념
same shape, nested loop, element-wise addition, result matrix.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic matrix_addition.c -o matrix_addition
```
## 실행 방법
```sh
./matrix_addition
```
## 예상 관찰 결과
예제 값을 사용하면 두 rows 모두 `7 7 7`이다.
## 확인 포인트
동일한 row와 column 위치끼리 더하는가?
## 추가 실습
- ★ 2x2 - ★★ 음수 포함 - ★★★ row 합
## 완료 기준
- [ ] 경고 없음 - [ ] 여섯 결과 정확 - [ ] shape 전제 설명
