# 30-3 실습: linked list 순회·검색
이론: [note](../../../notes/30-data-structures/30-3-linked-list-traversal-and-search.md)
## 실습 목적
읽기 전용 순회로 값을 찾고 NULL 종료를 처리한다.
## 작성할 파일
- `main.c`
## 해야 할 일
`typedef struct Node { int value; struct Node *next; } Node;`를 사용하고 `const Node *find(const Node *head, int target)`를 구현한다. node를 수정하지 않고 첫 일치 node 또는 `NULL`을 반환하며 caller는 반환값을 해제하지 않는다.
## 사용할 개념
`const`, `NULL`, lifetime, 비순환 단일 연결, `O(n)` (`n`은 현재 node 수).
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o search
```
## 실행 방법
```sh
./search
```
## 예상 관찰 결과
| 입력 | 기대 결과 |
|---|---|
| 빈 목록 | 미발견 |
| 한 node의 일치·불일치 | 첫 node·미발견 |
| 여러 node의 첫·중간·마지막 | 각 위치의 node |
| 여러 node의 없는 값 | 미발견 |
## 확인 포인트
`NULL` 확인 전에는 멤버를 읽지 않는다. 첫 일치의 포인터가 기존 node를 가리키며 원래 소유자가 lifetime을 책임진다. pointer chasing/cache는 CPU 성능이지 C17 보장이 아니다.
## 추가 실습
- ★ 방문 횟수를 출력한다.
- ★★ 중복 값에서 처음 발견한 위치를 확인한다.
- ★★★ 빈·단일·다중 목록의 최악 방문 수를 `n`으로 기술한다.
## 완료 기준
strict build와 빈·단일·다중·첫·중간·마지막·미발견 검색이 모두 통과하고 목록의 연결을 바꾸지 않는다.
