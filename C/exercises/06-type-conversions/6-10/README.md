# 6-10 실습: Part 6 종합 복습
이론: [note](../../../notes/06-type-conversions/6-10-part-6-review.md)
## 실습 목적
변환 순서와 경계를 종합한다.
## 작성할 파일
`part6_review.c`
## 해야 할 일
unsigned 축소, cast 평균, 음수 정수 나눗셈을 출력한다.
## 사용할 개념
promotion, 공통형, cast, 절단, 범위.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic part6_review.c -o part6_review
```
## 실행 방법
```sh
./part6_review
```
## 예상 관찰 결과
0, 2.5, -2.
## 확인 포인트
- 각 중간형을 기록했는가?
- UB를 실행하지 않았는가?
## 추가 실습
- ★ 기초: 변환표.
- ★★ 응용: 공통형 추적.
- ★★★ 도전: 경계표.
## 완료 기준
- [ ] 경고 없이 컴파일된다.
- [ ] 세 결과를 설명했다.
- [ ] Part 7 파일을 만들지 않았다.
- [ ] 답안 소스를 제공하지 않았다.

## 다음 Step
Step 7-1. 식·피연산자·결과값
