# 10-11 실습: Part 10 종합 복습

이론: [note](../../../notes/10-functions/10-11-part-10-review.md)
## 실습 목적
함수 선언·정의·호출·값 전달·반환을 한 프로그램에서 종합한다.
## 작성할 파일
`part10_review.c`
## 해야 할 일
`sum_to(int)` prototype과 definition을 작성하고 `main`에서 5를 전달해 결과를 출력한다.
## 사용할 개념
prototype, parameter, argument, local automatic object, return value, caller.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic part10_review.c -o part10_review
```
## 실행 방법
```sh
./part10_review
```
## 예상 관찰 결과
1부터 5까지의 합 `15`가 출력된다.
## 확인 포인트
prototype과 definition 타입이 일치하고 반환값을 caller가 사용하는가?
## 추가 실습
- ★ 홀짝 함수 - ★★ factorial 함수 - ★★★ 개념 점검표
## 완료 기준
- [ ] 경고 없음 - [ ] 결과 15 - [ ] 핵심 용어 설명 - [ ] Part 11 파일 미생성
## 다음 Step
Step 11-1. 배열 선언과 초기화
