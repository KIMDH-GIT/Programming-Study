# 5-14 실습: 두 수의 사칙연산

이론: [5-14 note](../../../notes/05-input-output/5-14-two-number-arithmetic.md)

## 실습 목적
두 정수의 계산과 출력을 연결한다.
## 작성할 파일
`two_number_arithmetic.c`
## 해야 할 일
제수가 0이 아닌 고정 정수 둘의 합·차·곱·몫을 출력한다.
## 사용할 개념
정수식, 사칙연산, `%d`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic two_number_arithmetic.c -o two_number_arithmetic
```
## 실행 방법
```sh
./two_number_arithmetic
```
## 예상 관찰 결과
네 계산 결과가 표시된다.
## 확인 포인트
- 제수가 0이 아닌가?
- 정수 몫을 예상했는가?
## 추가 실습
- ★ **기초:** 값을 바꾼다.
- ★★ **응용:** 나머지를 추가한다.
- ★★★ **도전:** 입력 검증을 설계한다.
## 완료 기준
- [ ] 경고 없이 컴파일된다.
- [ ] 네 결과가 맞다.
- [ ] 0 제수 위험을 설명했다.
- [ ] 답안 소스를 제공하지 않았다.
