# 7-1 실습: 식과 피연산자
이론: [note](../../../notes/07-operators/7-1-expressions-operands-results.md)
## 실습 목적
식의 구성요소와 결과형을 구별한다.
## 작성할 파일
`expressions.c`
## 해야 할 일
`int + double` 식을 작성하고 구성요소 표를 만든다.
## 사용할 개념
operator, operand, expression, result type.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic expressions.c -o expressions
```
## 실행 방법
```sh
./expressions
```
## 예상 관찰 결과
7.5.
## 확인 포인트
- 변환 뒤 결과형을 적었는가?
- statement와 구별했는가?
## 추가 실습
- ★ 기초: 정수식.
- ★★ 응용: 혼합식.
- ★★★ 도전: 하위 식 표.
## 완료 기준
- [ ] 경고 없이 컴파일된다.
- [ ] 구성요소를 설명했다.
- [ ] 답안 소스를 제공하지 않았다.
