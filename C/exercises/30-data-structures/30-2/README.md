# 30-2 실습: node 생성과 소유권
이론: [note](../../../notes/30-data-structures/30-2-node-creation-and-ownership.md)
## 실습 목적
node를 할당·초기화하고 소유자의 해제 책임을 확인한다.
## 작성할 파일
- `main.c`
## 해야 할 일
`typedef struct Node { int value; struct Node *next; } Node;`와 생성 함수를 작성한다. `Node *node = malloc(sizeof *node);`로 할당하고 `NULL`이면 기존 구조를 건드리지 않고 실패를 반환한다. 성공할 때만 멤버를 초기화하고 caller에게 소유권을 넘긴다. 생성된 모든 node를 정확히 한 번 해제한다.
## 사용할 개념
`malloc`, `free`, 미완성 `struct Node *`, 선언 후 typedef `Node`·`sizeof(Node)`, lifetime, 얕은 복사.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o create_node
```
## 실행 방법
```sh
./create_node
```
## 예상 관찰 결과
정상 생성된 단일 node는 값과 `next == NULL`을 보인다. 할당 실패 경로는 연결이 그대로 남고 해제할 새 node도 없다.
## 확인 포인트
빈 목록, 한 node, 여러 node에서 첫·중간·마지막 값을 확인하고 없는 값은 끝에서 미발견으로 처리한다. 할당 실패를 확인하기 전 기존 `head`나 `next`를 바꾸지 않는다. 포인터·구조체 복사로 새 소유자가 생기지 않으며 메모리 주소 배치를 가정하지 않는다.
## 추가 실습
- ★ 한 node를 생성하고 해제한다.
- ★★ 여러 node의 소유자를 추적하며 첫·중간·마지막을 해제한다.
- ★★★ 실패 경로와 얕은 복사 시나리오를 그려 중복 해제가 일어나지 않는 계약을 쓴다.
## 완료 기준
strict build가 통과하고 빈·단일·다중·첫·중간·마지막·미발견 및 할당 실패를 설명하며 모든 성공한 할당을 정확히 한 번 해제한다.
