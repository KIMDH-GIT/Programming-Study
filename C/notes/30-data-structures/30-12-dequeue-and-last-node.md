# 30-12. `dequeue`와 마지막 node
## 1. 학습 목표
- status와 output pointer로 빈 queue를 처리한다.
- 마지막 node 제거 후 head와 tail을 모두 NULL로 돌린다.
- node를 해제하기 전에 필요한 값을 저장한다.
## 2. 선수 지식
30-11의 linked queue 불변식과 `enqueue`를 안다.
## 3. 핵심 개념
**[ADT / operation]** `queue_dequeue(Queue *queue, int *value)`는 성공 시 1과 맨 앞 값을 반환하고, NULL 인자 또는 빈 queue에서 0을 반환하며 출력 인자는 변경하지 않는다. 값 -1도 정상 데이터이므로 sentinel로 쓰지 않는다.

non-NULL output은 호출과 반환까지 lifetime이 유지되는 writable `int` 객체를 가리키며 `Queue` 객체나 queue가 소유한 어떤 node와도 겹치지 않아야 한다.

**[자료구조]** empty는 head/tail NULL, size 0; nonempty는 acyclic/NULL-terminated chain, head/tail non-NULL, tail->next NULL, size가 chain 길이와 같다. `n`은 현재 저장된 수다.
## 4. 문법
```c
typedef struct { Node *head; Node *tail; size_t size; } Queue;
/* 성공한 경우에만 *value를 갱신한다. */
static int queue_dequeue(Queue *queue, int *value);
```
## 5. 최소 코드 예제
```c
#include <stdint.h>
#include <stddef.h>
#include <stdio.h>
#include <stdlib.h>

typedef struct Node { int value; struct Node *next; } Node;
typedef struct { Node *head; Node *tail; size_t size; } Queue;

/* Contract: valid queue and size < SIZE_MAX. */
static int queue_enqueue(Queue *queue, int value)
{
    Node *node = malloc(sizeof *node);
    if (node == NULL) return 0;
    node->value = value;
    node->next = NULL;
    if (queue->head == NULL) queue->head = node;
    else queue->tail->next = node;
    queue->tail = node;
    ++queue->size;
    return 1;
}

static int queue_dequeue(Queue *queue, int *value)
{
    if (queue == NULL || value == NULL || queue->head == NULL) return 0;
    Node *old = queue->head;
    int removed = old->value;
    Node *next = old->next;
    queue->head = next;
    --queue->size;
    if (next == NULL) queue->tail = NULL;
    *value = removed;
    free(old);
    return 1;
}

/* Precondition: queue is non-NULL and points to a live writable Queue
   satisfying its ownership and chain invariant. An empty queue is allowed. */
static void queue_destroy(Queue *queue)
{
    Node *node = queue->head;
    while (node != NULL) {
        Node *next = node->next;
        free(node);
        node = next;
    }
    queue->head = queue->tail = NULL;
    queue->size = 0;
}

int main(void)
{
    Queue queue = {NULL, NULL, 0};
    int value = 77;
    printf("empty=%d value=%d\n", queue_dequeue(&queue, &value), value);
    if (!queue_enqueue(&queue, -1)) return 1;
    if (!queue_dequeue(&queue, &value)) { queue_destroy(&queue); return 1; }
    printf("single=%d empty=%d\n", value,
           queue.head == NULL && queue.tail == NULL && queue.size == 0);
    if (!queue_enqueue(&queue, 10) || !queue_enqueue(&queue, 20)) {
        queue_destroy(&queue);
        return 1;
    }
    while (queue_dequeue(&queue, &value)) printf("out=%d\n", value);
    if (!queue_enqueue(&queue, 30)) return 1;
    if (!queue_dequeue(&queue, &value)) { queue_destroy(&queue); return 1; }
    printf("reuse=%d size=%zu\n", value, queue.size);
    queue_destroy(&queue);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o queue_dequeue
./queue_dequeue
```
## 6. 코드 해석
`empty=0 value=77`, `single=-1 empty=1`, `out=10`, `out=20`, `reuse=30 size=0` 순으로 출력된다. 빈 호출에서는 출력값 77을 건드리지 않는다.
## 7. 내부 동작
**[자료구조 / ADT]** 선두를 제거하면 다음 node가 head가 된다. next가 NULL이면 마지막 node였으므로 tail도 NULL로 만든다. 포인터 재배선은 O(1)이며 FIFO 출력 순서를 유지한다.

**[C17 구현]** old가 유효할 때 value와 next를 보관하고, head와 size를 갱신한 뒤 free한다. free 이후 old를 읽지 않는다. `enqueue`의 size 증가는 `size < SIZE_MAX` 계약 아래에서 수행한다.

**[allocator 구현 / OS / CPU]** free 시각과 실제 메모리 반환 시점은 allocator/OS의 문제이며 O(1)은 wall-clock 또는 CPU instruction 수의 보장이 아니다.
## 8. 자주 하는 실수
- 마지막 node를 제거한 뒤 tail을 해제된 node에 둔다.
- free(old) 이후 old->next 또는 old->value를 읽는다.
- 빈 상태를 -1 데이터와 혼동한다.
- 부분 삽입 실패 후 기존 node를 해제하지 않는다.
## 9. 필수 실습
빈 상태, 한 node 제거, 여러 node 전부 제거, 빈 상태에서 다시 삽입을 검사한다.
[30-12 exercise](../../exercises/30-data-structures/30-12/README.md)
## 10. 추가 실습
- ★ NULL 인자에서 status와 출력 보존을 확인한다.
- ★★ -1 데이터를 정상적으로 꺼낸다.
- ★★★ 중간 삽입 실패 후 기존 node를 순서대로 비우고 재사용한다.
## 11. 확인 문제
1. 출력 인자를 왜 success에서만 갱신하는가?
2. next가 NULL이면 어떤 필드를 추가로 바꾸어야 하는가?
3. free 이전에 저장해야 하는 두 값은 무엇인가?
4. `-1` sentinel은 어떤 입력에서 실패하는가?
5. 전체를 비운 뒤 다시 enqueue할 수 있는 이유는?
## 12. 핵심 정리
- `dequeue`는 status와 출력값을 분리한다.
- 마지막 제거 시 head/tail/size가 동시에 빈 상태를 나타낸다.
- 해제한 node를 더는 참조하지 않는다.
## 13. 다음 Step
[30-13. allocation 실패와 구조 보존](30-13-allocation-failure-and-state-preservation.md)
## 14. 참고 자료
- N1570 6.2.4, 7.22.3.3, 7.22.3.4. N1570은 C11 공개 Committee Draft이며 사용한 규칙은 C17에서도 유지된다.
- Open Data Structures, Pat Morin, §3.2 `SLList`: https://opendatastructures.org/ods-cpp/3_2_SLList_Singly_Linked_Li.html
