# 10-8 실습: call stack 기초

이론: [note](../../../notes/10-functions/10-8-call-stack-basics.md)
## 실습 목적
caller에서 called function으로 갔다가 돌아오는 순서를 관찰한다.
## 작성할 파일
`call_flow.c`
## 해야 할 일
호출 전, 함수 내부, 반환 후에 서로 다른 문장을 출력한다.
## 사용할 개념
caller, called function, return, call stack 구현 모델.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic call_flow.c -o call_flow
```
## 실행 방법
```sh
./call_flow
```
## 예상 관찰 결과
호출 전 → 함수 내부 → 반환 후 순서로 출력된다.
## 확인 포인트
C17 제어 흐름과 ABI의 stack frame 설명을 구분했는가?
## 추가 실습
- ★ 두 함수 호출 - ★★ A에서 B 호출 - ★★★ 표준/ABI 표
## 완료 기준
- [ ] 경고 없음 - [ ] 출력 순서 정확 - [ ] stack 단정 없음
