# 6-8 실습: 정수와 실수 나눗셈
이론: [note](../../../notes/06-type-conversions/6-8-integer-real-division.md)
## 실습 목적
피연산자형과 나눗셈 결과를 연결한다.
## 작성할 파일
`division_types.c`
## 해야 할 일
`5/2`, `5.0/2`, `-5/2`를 출력한다.
## 사용할 개념
공통형, 0 방향 절단.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic division_types.c -o division_types
```
## 실행 방법
```sh
./division_types
```
## 예상 관찰 결과
2, 2.5, -2.
## 확인 포인트
- 0 방향 절단을 설명했는가?
- UB 경계를 실행하지 않았는가?
## 추가 실습
- ★ 기초: 7/2.
- ★★ 응용: -7/2.
- ★★★ 도전: 수직선.
## 완료 기준
- [ ] 경고 없이 컴파일된다.
- [ ] 세 결과를 설명했다.
- [ ] 답안 소스를 제공하지 않았다.
