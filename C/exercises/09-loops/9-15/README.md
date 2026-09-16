# 9-15 실습: 범위 내 모든 소수

이론: [note](../../../notes/09-loops/9-15-primes-in-range.md)
## 실습 목적
후보와 약수 검사를 중첩해 범위 결과를 만든다.
## 작성할 파일
`primes_in_range.c`
## 해야 할 일
2부터 20까지 모든 소수를 한 줄에 하나씩 출력한다.
## 사용할 개념
중첩 `for`, flag, `%`, 내부 `break`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic primes_in_range.c -o primes_in_range
```
## 실행 방법
```sh
./primes_in_range
```
## 예상 관찰 결과
2, 3, 5, 7, 11, 13, 17, 19가 출력된다.
## 확인 포인트
후보마다 flag가 다시 1이 되고 1은 출력되지 않는가?
## 추가 실습
- ★ 2~10 - ★★ 10~30 - ★★★ 검사 횟수 표시
## 완료 기준
- [ ] 경고 없음 - [ ] 8개 소수 정확 - [ ] 배열 사용 없음
