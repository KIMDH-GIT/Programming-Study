# 9-11 실습: 구구단

이론: [note](../../../notes/09-loops/9-11-multiplication-table.md)
## 실습 목적
고정값과 반복 변수를 조합해 한 단을 출력한다.
## 작성할 파일
`multiplication_table.c`
## 해야 할 일
2단을 `2 x 1 = 2` 형식으로 9까지 출력한다.
## 사용할 개념
`for`, 곱셈, 포함 범위, `printf`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic multiplication_table.c -o multiplication_table
```
## 실행 방법
```sh
./multiplication_table
```
## 예상 관찰 결과
`2 x 1 = 2`부터 `2 x 9 = 18`까지 9줄이다.
## 확인 포인트
승수 1과 9가 포함되고 서식 인자 순서가 맞는가?
## 추가 실습
- ★ 5단 - ★★ 역순 - ★★★ 2~4단
## 완료 기준
- [ ] 경고 없음 - [ ] 9줄 정확 - [ ] 계산 결과 정확
