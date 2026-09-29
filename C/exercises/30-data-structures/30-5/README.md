# 30-5 실습: linked list delete와 head 갱신
이론: [note](../../../notes/30-data-structures/30-5-linked-list-delete-and-head-update.md)
## 실습 목적
첫 일치 node를 안전하게 제거하고 caller의 첫 포인터를 갱신한다.
## 작성할 파일
- `main.c`
## 해야 할 일
`typedef struct Node { int value; struct Node *next; } Node;`를 사용한다. `Node **head`와 `int *out`을 받아 첫 일치 node를 삭제한다. 두 인자는 NULL이 아니며, `head`는 owned chain 밖의 살아 있는 writable caller-owned `Node *` 객체를 가리키고 `*head == NULL`은 유효한 빈 목록이다. `out`은 호출과 반환까지 lifetime이 유지되는 writable `int` 객체를 가리키며 첫 포인터 객체나 목록이 소유한 어떤 node와도 겹치지 않는다. 다음 링크와 삭제 값을 해제 전에 저장하고 연결·head를 갱신한 뒤 `free`한다. 성공일 때만 output을 기록하고 미발견은 status 0으로 표현한다. `prepend`와 `clear`도 같은 non-NULL `head` precondition을 사용하며 빈 목록은 허용한다.
## 사용할 개념
`Node **`, 첫 포인터 갱신, `NULL`, lifetime, `free`, status/output, 소유권.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o delete
```
## 실행 방법
```sh
./delete
```
## 예상 관찰 결과
| 시작 상태·대상 | 기대 결과 |
|---|---|
| 빈 목록·없는 값 | status 0, head·output 유지 |
| 한 node·그 값 | status 1, head NULL |
| 한 node·없는 값 | status 0, 연결 유지 |
| 여러 node·첫·중간·마지막 | 각 첫 일치만 제거, 나머지 순서 유지 |
| 여러 node·없는 값 | status 0, head·output 유지 |
## 확인 포인트
`free` 후 제거한 node를 읽지 않는다. 마지막 제거 후 `next == NULL`이며 첫 제거 후 caller의 head가 새 첫 node를 가리킨다. `-1` 같은 값 sentinel로 실패를 나타내지 않는다.
## 추가 실습
- ★ 단일 node 삭제 전후의 head를 확인한다.
- ★★ 첫·중간·마지막 및 미발견 상태를 각각 검사한다.
- ★★★ 중복 값의 첫 일치만 삭제하고 남은 node들을 해제한다.
## 완료 기준
strict build와 빈·단일·다중·첫·중간·마지막·미발견·단일에서 빈 목록 전환 검사가 통과하며 유효하지 않은 해제 후 접근이 없다.
