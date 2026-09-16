# 7-9 실습: 묶임과 평가 순서
이론: [note](../../../notes/07-operators/7-9-precedence-associativity-order.md)
## 실습 목적
우선순위와 괄호 효과를 확인한다.
## 작성할 파일
`precedence.c`
## 해야 할 일
괄호 없는 식과 괄호 식을 출력한다.
## 사용할 개념
precedence, associativity, evaluation order.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic precedence.c -o precedence
```
## 실행 방법
```sh
./precedence
```
## 예상 관찰 결과
14와 20.
## 확인 포인트
- 묶임과 실행 순서를 구별했는가?
- side effect를 넣지 않았는가?
## 추가 실습
- ★ 기초: 산술 묶임.
- ★★ 응용: 대입 결합.
- ★★★ 도전: sequencing 표.
## 완료 기준
- [ ] 경고 없이 컴파일된다.
- [ ] 두 결과를 설명했다.
- [ ] 답안 소스를 제공하지 않았다.
