# 8-1 실습: if
이론: [note](../../../notes/08-conditionals/8-1-if.md)
## 실습 목적
조건과 controlled statement를 구별한다.
## 작성할 파일
`if_basic.c`
## 해야 할 일
양수 조건에서 메시지를 출력한다.
## 사용할 개념
`if`, 비교, compound statement.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic if_basic.c -o if_basic
```
## 실행 방법
```sh
./if_basic
```
## 예상 관찰 결과
positive.
## 확인 포인트
- 중괄호를 사용했는가?
- 0/비0를 설명했는가?
## 추가 실습
- ★ 0. - ★★ 음수. - ★★★ 대입 조건 분석.
## 완료 기준
- [ ] 경고 없이 컴파일된다.
- [ ] 조건 흐름을 설명했다.
- [ ] 답안 소스를 제공하지 않았다.
