# 7-8 실습: 위험한 변경식 분해
이론: [note](../../../notes/07-operators/7-8-unsequenced-modification.md)
## 실습 목적
UB 식을 실행하지 않고 안전하게 분해한다.
## 작성할 파일
`sequenced_changes.c`와 분석표
## 해야 할 일
각 변경을 별도 statement로 작성한다.
## 사용할 개념
full expression, side effect, UB.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic sequenced_changes.c -o sequenced_changes
```
## 실행 방법
```sh
./sequenced_changes
```
## 예상 관찰 결과
6과 8.
## 확인 포인트
- UB 원문을 실행하지 않았는가?
- 변경 순서가 명확한가?
## 추가 실습
- ★ 기초: 단일 변경.
- ★★ 응용: 식 분해.
- ★★★ 도전: 규칙 조사.
## 완료 기준
- [ ] 안전 코드만 실행했다.
- [ ] UB 이유를 설명했다.
- [ ] 답안 소스를 제공하지 않았다.
