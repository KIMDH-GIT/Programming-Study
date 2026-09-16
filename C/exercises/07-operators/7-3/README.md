# 7-3 실습: 나눗셈 경계 분석
이론: [note](../../../notes/07-operators/7-3-division-boundaries.md)
## 실습 목적
안전한 경계와 UB를 구별한다.
## 작성할 파일
`division_boundaries.c`와 분석표
## 해야 할 일
`INT_MIN+1`의 안전한 나눗셈만 실행하고 위험 식은 표로 분석한다.
## 사용할 개념
0 제수, 표현 불가능한 몫, UB.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic division_boundaries.c -o division_boundaries
```
## 실행 방법
```sh
./division_boundaries
```
## 예상 관찰 결과
`INT_MAX`가 표시된다.
## 확인 포인트
- UB 식을 실행하지 않았는가?
- CPU 결과로 일반화하지 않았는가?
## 추가 실습
- ★ 기초: 안전 제수.
- ★★ 응용: 경계표.
- ★★★ 도전: 최적화 조사.
## 완료 기준
- [ ] 안전 코드만 실행했다.
- [ ] 두 UB를 설명했다.
- [ ] 답안 소스를 제공하지 않았다.
