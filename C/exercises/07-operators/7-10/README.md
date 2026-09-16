# 7-10 실습: Part 7 종합 복습
이론: [note](../../../notes/07-operators/7-10-part-7-review.md)
## 실습 목적
연산자 규칙과 UB 경계를 종합한다.
## 작성할 파일
`part7_review.c`와 판별표
## 해야 할 일
안전 guard, 복합 대입, 비교를 실행하고 위험 식은 표로만 분석한다.
## 사용할 개념
conversion, result type, short-circuit, side effect, UB.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic part7_review.c -o part7_review
```
## 실행 방법
```sh
./part7_review
```
## 예상 관찰 결과
1, 8, 1.
## 확인 포인트
- UB를 실행하지 않았는가?
- 평가 순서를 과장하지 않았는가?
## 추가 실습
- ★ 기초: 분류표.
- ★★ 응용: 결과형 추적.
- ★★★ 도전: UB 판별표.
## 완료 기준
- [ ] 경고 없이 컴파일된다.
- [ ] 세 결과를 설명했다.
- [ ] Part 8 파일을 만들지 않았다.
- [ ] 답안 소스를 제공하지 않았다.

## 다음 Step
Step 8-1. `if`
