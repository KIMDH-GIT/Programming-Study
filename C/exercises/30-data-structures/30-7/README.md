# 30-7 실습: linked list destroy
이론: [note](../../../notes/30-data-structures/30-7-linked-list-destroy.md)
## 실습 목적
node 소유권을 끝내고 호출자의 head를 `NULL`로 되돌린다.
## 작성할 파일
- `main.c`
## 해야 할 일
`list_destroy(Node **head)`에서 해제 전에 `next`를 보관하고 각 node를 한 번씩 해제한다. `head == NULL`과 `*head == NULL`을 허용한다. `malloc` 실패 시 이미 만든 chain도 정리한다.
## 사용할 개념
pointer-to-pointer, `malloc`, `free`, object lifetime, ownership.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o list_destroy
```
## 실행 방법
```sh
./list_destroy
```
## 예상 관찰 결과
정상 할당에서 세 node `3 -> 2 -> 1`을 먼저 출력한 후 destroy 두 번과 단일 node destroy 결과는 정확히 다음과 같다.
```text
before=3,2,1
after=1
single=1
```

| 입력 상태 | 호출 후 | 경계 |
|---|---|---|
| `head == NULL` | 아무 동작 없음 | NULL 인자 |
| `*head == NULL` | `*head == NULL` | 빈 chain, 반복 호출 |
| 한 node | `*head == NULL` | 정확히 한 번 해제 |
| 세 node | `*head == NULL` | 모든 node 한 번씩 해제 |
| 생성 중 할당 실패 | 소유한 부분 chain 정리, 종료 상태 1 | 누수 방지 |
## 확인 포인트
해제한 node에서 `next`를 읽지 않는다. memory leak은 해제하지 못한 소유 객체, dangling pointer는 lifetime이 끝난 객체에 대한 남은 pointer이다. 다른 alias는 destroy가 지우지 않는다.
## 추가 실습
- ★ 빈 chain과 단일 node를 시험한다.
- ★★ 할당 실패 경로에서 부분 chain을 해제한다.
- ★★★ 외부 alias를 사용하지 않아야 하는 이유를 설명한다.
## 완료 기준
각 node를 한 번씩만 해제하고 빈·단일·다중·반복 destroy와 실패 경로에서 head가 `NULL`이다.
