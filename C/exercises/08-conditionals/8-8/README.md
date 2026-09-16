# 8-8 실습
이론: [note](../../../notes/08-conditionals/8-8-grade-calculation.md)
## 실습 목적
학점 경계를 구현한다.
## 작성할 파일
`grade.c`
## 해야 할 일
고정 점수를 A/B/C/F로 분류한다.
## 사용할 개념
else if, 경계, char.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic grade.c -o grade
```
## 실행 방법
```sh
./grade
```
## 예상 관찰 결과
A.
## 확인 포인트
높은 기준부터인가?
## 추가 실습
- ★ 90. - ★★ 79. - ★★★ 범위 검증.
## 완료 기준
- [ ] 경고 없음. - [ ] 경계 설명. - [ ] 답안 없음.
