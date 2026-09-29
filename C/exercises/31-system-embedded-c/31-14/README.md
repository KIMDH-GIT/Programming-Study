# 31-14 실습: callback context ownership·lifetime
이론: [note](../../../notes/31-system-embedded-c/31-14-callback-context-ownership-and-lifetime.md)
## 실습 목적
즉시 호출 callback에서 context lifetime과 borrow contract를 검증한다.
## 작성할 파일
- `main.c`
## 해야 할 일
live automatic context를 두 번 즉시 dispatch한다. dispatch는 callback/context를 저장하지 않는다. 장기 등록 또는 ISR/thread 호출로 확장하지 않는다.
## 사용할 개념
callback, context, owner, borrow, object lifetime, storage duration.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o callback_context
```
## 실행 방법
```sh
./callback_context
```
## 예상 관찰 결과
`sum=7`을 출력한다.
## 확인 포인트
context lifetime이 모든 callback use를 포함한다. 같은 address 재사용을 같은 object lifetime으로 보지 않는다.
## 추가 실습
- ★ NULL callback을 no-op으로 처리한다.
- ★★ owner/borrow/저장 여부 표를 만든다.
- ★★★ unregister가 있는 장기 등록 contract를 설계만 한다.
## 완료 기준
expired context 없이 정확한 결과가 나오고 lifetime contract를 설명한다.
