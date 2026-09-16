# 8-3 실습: else if
이론: [note](../../../notes/08-conditionals/8-3-else-if.md)
## 실습 목적
세 구간 중 한 경로를 선택한다.
## 작성할 파일
`else_if.c`
## 해야 할 일
점수 고정값을 A/B/C로 분류한다.
## 사용할 개념
조건 순서, first true branch.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic else_if.c -o else_if
```
## 실행 방법
```sh
./else_if
```
## 예상 관찰 결과
B.
## 확인 포인트
- 높은 기준부터 검사하는가?
- 한 경로만 실행되는가?
## 추가 실습
- ★ 90. - ★★ 79. - ★★★ 순서 분석.
## 완료 기준
- [ ] 경고 없이 컴파일된다.
- [ ] 경계를 설명했다.
- [ ] 답안 소스를 제공하지 않았다.
