# 30-4 실습: linked list insert
이론: [note](../../../notes/30-data-structures/30-4-linked-list-insert.md)
## 실습 목적
할당 성공 후 앞·중간·끝에 node를 연결한다.
## 작성할 파일
- `main.c`
## 해야 할 일
`typedef struct Node { int value; struct Node *next; } Node;`로 NULL 종료·비순환 목록을 표현한다. `Node **head`를 받아 앞에 삽입하고, 알려진 이전 node 뒤에 삽입하는 동작도 구현한다. `head`는 NULL이 아니며 owned chain 밖에 있는 살아 있는 writable caller-owned `Node *` 객체를 가리키고, `*head == NULL`은 유효한 빈 목록이다. 모든 할당에 `NULL` 처리를 넣고 실패 시 기존 `head`·연결·소유권을 보존한다. 마지막에 성공한 할당만 정확히 한 번씩 해제한다.
## 사용할 개념
`Node **`, pointer-by-value, `malloc(sizeof *node)`, 링크 변경, `free`, `O(1)` 재연결과 `O(n)` 검색+삽입.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o insert
```
## 실행 방법
```sh
./insert
```
## 예상 관찰 결과
빈 목록에 앞 삽입하면 단일 node가 되고, 기존 첫·중간·마지막 뒤 또는 앞에 삽입하면 값의 순서가 예상대로 된다. 없는 위치를 검색한 경우 삽입하지 않는다.
## 확인 포인트
빈·단일·다중 목록, 첫·중간·끝 삽입, 위치 미발견, 할당 실패를 확인한다. 단일 목록의 뒤 삽입 뒤에도 끝은 `NULL`이고 실패 시 첫 포인터가 그대로다.
## 추가 실습
- ★ 두 번 앞에 삽입해 순서를 확인한다.
- ★★ 알려진 이전 node의 뒤에 삽입한다.
- ★★★ 검색 결과가 없는 경우와 할당 실패 경로에서 원래 연결이 유지됨을 확인한다.
## 완료 기준
strict build와 모든 경계 검사가 통과하고 목록의 성공한 node를 누락·중복 없이 해제한다.
