# 8-9 실습
이론: [note](../../../notes/08-conditionals/8-9-menu-calculator.md)
## 실습 목적
메뉴로 안전한 연산을 선택한다.
## 작성할 파일
`menu_calculator.c`
## 해야 할 일
고정 메뉴와 두 수로 한 연산을 출력한다.
## 사용할 개념
switch, break, default, 0 제수 guard.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic menu_calculator.c -o menu_calculator
```
## 실행 방법
```sh
./menu_calculator
```
## 예상 관찰 결과
선택 연산 결과.
## 확인 포인트
break와 guard가 있는가?
## 추가 실습
- ★ 덧셈. - ★★ 나눗셈. - ★★★ invalid.
## 완료 기준
- [ ] 경고 없음. - [ ] 안전 분기. - [ ] 답안 없음.
