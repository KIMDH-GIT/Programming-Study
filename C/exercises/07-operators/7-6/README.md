# 7-6 실습: 대입 연산자
이론: [note](../../../notes/07-operators/7-6-assignment-operators.md)
## 실습 목적
초기화와 대입을 구별하고 복합 대입을 사용한다.
## 작성할 파일
`assignment_operators.c`
## 해야 할 일
안전한 값으로 여섯 대입 연산을 적용한다.
## 사용할 개념
assignment, initialization, compound assignment.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic assignment_operators.c -o assignment_operators
```
## 실행 방법
```sh
./assignment_operators
```
## 예상 관찰 결과
최종값 6.
## 확인 포인트
- 제수가 0이 아닌가?
- 초기화와 대입을 구별했는가?
## 추가 실습
- ★ 기초: 덧셈 대입.
- ★★ 응용: 곱셈 대입.
- ★★★ 도전: 변환 분석.
## 완료 기준
- [ ] 경고 없이 컴파일된다.
- [ ] 결과를 설명했다.
- [ ] 답안 소스를 제공하지 않았다.
