# 9-12 실습: factorial

이론: [note](../../../notes/09-loops/9-12-factorial.md)
## 실습 목적
곱셈 누적과 0! 경계를 이해한다.
## 작성할 파일
`factorial.c`
## 해야 할 일
상수 5의 factorial을 반복문으로 계산해 출력한다. overflow 가능한 큰 값은 실행하지 않는다.
## 사용할 개념
곱셈 누적, 항등값 1, 포함 범위, signed overflow.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic factorial.c -o factorial
```
## 실행 방법
```sh
./factorial
```
## 예상 관찰 결과
`120`이 출력된다.
## 확인 포인트
누적값이 1이고 factor가 2부터 시작하는가?
## 추가 실습
- ★ 3! - ★★ 0! 추적 - ★★★ 안전 범위 조사
## 완료 기준
- [ ] 경고 없음 - [ ] 결과 120 - [ ] overflow 코드 실행 안 함
