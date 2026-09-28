# 29-11 실습: Prime Test
이론: [note](../../../notes/29-basic-algorithms/29-11-prime-test.md)
## 실습 목적
small boundaries와 square-root divisor 범위로 primality를 판정한다.
## 작성할 파일
- `main.c`
## 해야 할 일
0·1을 먼저 제외하고 overflow-safe bound로 divisor를 검사한다.
## 사용할 개념
prime, divisor, perfect square, arithmetic range, `O(sqrt(n))`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o prime_test
```
## 실행 방법
```sh
./prime_test
```
## 예상 관찰 결과
1과 30은 false, 2와 29는 true다.

| value | 기대 결과 | 확인 경계 |
|---:|---|---|
| 0 | false | below domain |
| 1 | false | boundary |
| 2 | true | smallest prime |
| 29 | true | odd prime |
| 30 | false | even composite |
| 49 | false | perfect square |
## 확인 포인트
`divisor <= value / divisor`가 product overflow를 피하는지 설명한다.
## 추가 실습
- ★ 97을 검사한다.
- ★★ odd divisors만 검사한다.
- ★★★ 작은 범위에서 reference trial division과 비교한다.
## 완료 기준
boundary·prime·composite·square cases가 모두 일치한다.
