# 9-14 실습: 소수 판별

이론: [note](../../../notes/09-loops/9-14-prime-test.md)
## 실습 목적
경계값과 약수 반복으로 소수를 판별한다.
## 작성할 파일
`prime_test.c`
## 해야 할 일
상수 17을 판별해 `prime`을 출력하고 18로 바꿔 결과를 비교한다.
## 사용할 개념
`for`, `%`, flag, short-circuit, 2 미만 경계.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic prime_test.c -o prime_test
```
## 실행 방법
```sh
./prime_test
```
## 예상 관찰 결과
17은 `prime`, 18은 `not prime`이다.
## 확인 포인트
1을 소수로 처리하지 않고 제수가 0이 되지 않는가?
## 추가 실습
- ★ 2 - ★★ 1 - ★★★ 첫 약수에서 종료
## 완료 기준
- [ ] 경고 없음 - [ ] 17/18 정확 - [ ] 경계 설명
