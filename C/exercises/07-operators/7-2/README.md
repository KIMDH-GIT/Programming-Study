# 7-2 실습: 산술 연산
이론: [note](../../../notes/07-operators/7-2-arithmetic-operators.md)
## 실습 목적
몫·나머지·복원 관계를 확인한다.
## 작성할 파일
`arithmetic_operators.c`
## 해야 할 일
안전한 정수 두 개로 몫, 나머지, 복원값을 출력한다.
## 사용할 개념
`/`, `%`, 0 방향 절단, 결과형.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic arithmetic_operators.c -o arithmetic_operators
```
## 실행 방법
```sh
./arithmetic_operators
```
## 예상 관찰 결과
-3, -2, -17.
## 확인 포인트
- 제수가 0이 아닌가?
- 복원식을 설명했는가?
## 추가 실습
- ★ 기초: 양수.
- ★★ 응용: 음수.
- ★★★ 도전: unsigned.
## 완료 기준
- [ ] 경고 없이 컴파일된다.
- [ ] 관계를 확인했다.
- [ ] 답안 소스를 제공하지 않았다.
