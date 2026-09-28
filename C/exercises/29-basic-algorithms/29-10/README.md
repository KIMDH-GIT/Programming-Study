# 29-10 실습: GCD
이론: [note](../../../notes/29-basic-algorithms/29-10-gcd.md)
## 실습 목적
Euclidean invariant를 유지하며 greatest common divisor를 구한다.
## 작성할 파일
- `main.c`
## 해야 할 일
`b != 0`일 때만 remainder를 계산하고 zero convention을 적용한다.
## 사용할 개념
GCD, modulo, invariant, progress, zero boundary.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o gcd
```
## 실행 방법
```sh
./gcd
```
## 예상 관찰 결과
`gcd(48,18)=6`, `gcd(0,7)=7`, `gcd(0,0)=0`이다.

| a | b | 기대 결과 | 확인 경계 |
|---:|---:|---:|---|
| 48 | 18 | 6 | normal |
| 7 | 7 | 7 | equal |
| 8 | 15 | 1 | coprime |
| 0 | 7 | 7 | one zero |
| 0 | 0 | 0 | application convention |
## 확인 포인트
각 iteration의 `(a,b)`와 나머지가 감소하는지 기록한다.
## 추가 실습
- ★ arguments를 바꿔 검증한다.
- ★★ iteration 수를 센다.
- ★★★ 큰 unsigned input의 remainder trace를 작성한다.
## 완료 기준
division by zero 없이 모든 vectors가 expected gcd를 만든다.
