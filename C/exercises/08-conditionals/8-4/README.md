# 8-4 실습: 중첩과 경계
이론: [note](../../../notes/08-conditionals/8-4-nested-boundaries.md)
## 실습 목적
중첩 경로와 경계 포함을 확인한다.
## 작성할 파일
`nested_boundaries.c`
## 해야 할 일
고정값을 음수/0..10/10 초과로 분류한다.
## 사용할 개념
중첩 if, 경계값, 중괄호.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic nested_boundaries.c -o nested_boundaries
```
## 실행 방법
```sh
./nested_boundaries
```
## 예상 관찰 결과
0..10.
## 확인 포인트
- 0과 10을 포함했는가?
- else 결합이 명확한가?
## 추가 실습
- ★ -1. - ★★ 11. - ★★★ 경계표.
## 완료 기준
- [ ] 경고 없이 컴파일된다.
- [ ] 네 경계를 기록했다.
- [ ] 답안 소스를 제공하지 않았다.
