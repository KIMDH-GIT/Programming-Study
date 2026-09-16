# 7-5 실습: 논리 연산과 short-circuit
이론: [note](../../../notes/07-operators/7-5-logical-short-circuit.md)
## 실습 목적
논리 결과와 오른쪽 평가 생략을 이해한다.
## 작성할 파일
`logical_short_circuit.c`
## 해야 할 일
0 제수를 안전하게 보호하는 논리식을 출력한다.
## 사용할 개념
0/비0 진릿값, `!`, `&&`, `||`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic logical_short_circuit.c -o logical_short_circuit
```
## 실행 방법
```sh
./logical_short_circuit
```
## 예상 관찰 결과
0, 1, 1.
## 확인 포인트
- UB 나눗셈이 평가되지 않는가?
- 결과형을 `int`로 설명했는가?
## 추가 실습
- ★ 기초: `!` 값.
- ★★ 응용: 진리표.
- ★★★ 도전: guard 분석.
## 완료 기준
- [ ] 경고 없이 컴파일된다.
- [ ] short-circuit를 설명했다.
- [ ] 답안 소스를 제공하지 않았다.
