# 7-7 실습: 증감 연산자
이론: [note](../../../notes/07-operators/7-7-increment-decrement.md)
## 실습 목적
전위·후위 결과와 side effect를 구별한다.
## 작성할 파일
`increment_decrement.c`
## 해야 할 일
각 연산을 별도 full expression에서 실행한다.
## 사용할 개념
prefix, postfix, side effect.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic increment_decrement.c -o increment_decrement
```
## 실행 방법
```sh
./increment_decrement
```
## 예상 관찰 결과
5, 7, 7.
## 확인 포인트
- 한 식에서 여러 번 변경하지 않았는가?
- 결과값과 저장값을 구별했는가?
## 추가 실습
- ★ 기초: 전위 증가.
- ★★ 응용: 후위 감소.
- ★★★ 도전: side effect 표.
## 완료 기준
- [ ] 경고 없이 컴파일된다.
- [ ] 차이를 설명했다.
- [ ] 답안 소스를 제공하지 않았다.
