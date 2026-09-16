# 9-17 실습: 중첩 반복문

이론: [note](../../../notes/09-loops/9-17-nested-loop-practice.md)
## 실습 목적
두 반복 변수의 조합과 전체 실행 횟수를 종합한다.
## 작성할 파일
`nested_loop_practice.c`
## 해야 할 일
1~3의 행과 열로 3x3 곱셈표를 출력한다.
## 사용할 개념
중첩 `for`, 독립된 경계, 곱셈, newline.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic nested_loop_practice.c -o nested_loop_practice
```
## 실행 방법
```sh
./nested_loop_practice
```
## 예상 관찰 결과
세 줄에 `1 2 3`, `2 4 6`, `3 6 9`가 출력된다.
## 확인 포인트
내부 변수가 외부 반복마다 1부터 다시 시작하는가?
## 추가 실습
- ★ 2x4 합 - ★★ 행 합 - ★★★ 결과 4 건너뛰기
## 완료 기준
- [ ] 경고 없음 - [ ] 3x3 결과 정확 - [ ] 내부 본문 9회
