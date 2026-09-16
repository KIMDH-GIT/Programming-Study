# 12-8 실습: 행렬 곱셈

이론: [note](../../../notes/12-two-dimensional-arrays/12-8-matrix-multiplication.md)
## 실습 목적
세 loops로 row와 column의 곱-누적을 구현한다.
## 작성할 파일
`matrix_multiplication.c`
## 해야 할 일
예제의 2x3과 3x2 matrix를 곱해 2x2 result를 출력한다.
## 사용할 개념
dimension compatibility, result shape, shared index, multiply-accumulate.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic matrix_multiplication.c -o matrix_multiplication
```
## 실행 방법
```sh
./matrix_multiplication
```
## 예상 관찰 결과
첫 row `58 64`, 둘째 row `139 154`가 출력된다.
## 확인 포인트
sum을 result element마다 한 번 초기화하고 shared dimension 3을 모두 누적하는가?
## 추가 실습
- ★ identity 곱셈 - ★★ 1x3·3x1 - ★★★ 누적표
## 완료 기준
- [ ] 경고 없음 - [ ] 2x2 결과 정확 - [ ] dimension 설명
