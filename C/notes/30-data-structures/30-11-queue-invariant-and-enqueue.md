# 30-11. queue 불변식과 `enqueue`
## 1. 학습 목표
- linked queue의 head, tail, size 불변식을 정의한다.
- 빈 queue와 비어 있지 않은 queue의 `enqueue`를 구분한다.
- 할당 실패 시 상태를 보존하고 O(1)의 의미를 설명한다.
## 2. 선수 지식
Part 30의 `Node`, linked list, linked stack에서 node의 소유권과 연결을 학습했다.
## 3. 핵심 개념
**[자료구조]** `Queue`는 FIFO 순서로 node를 연결한다. 빈 상태는 `head == NULL`, `tail == NULL`, `size == 0`이다. 비어 있지 않으면 head와 tail은 모두 NULL이 아니고, `tail->next == NULL`이며, head부터 NULL까지 이어지는 chain은 cycle이 없고 node 수가 size와 같다. 여기서 `n`은 현재 저장한 원소 수다.

**[ADT / operation]** `enqueue`는 새 값을 뒤에 넣고 성공 여부를 돌려준다. 성공 전에 queue를 수정하지 않는다. 계약상 유효한 `Queue *`를 받고, 호출 전 `size < SIZE_MAX`여야 증가가 표현 가능하다. 할당 실패에는 0을 반환하고 기존 순서와 포인터, size를 보존한다.
## 4. 문법
```c
typedef struct Node {
    int value;
    struct Node *next;
} Node;
typedef struct {
    Node *head;
    Node *tail;
    size_t size;
} Queue;
```
## 5. 최소 코드 예제
```c
#include <stdint.h>
#include <stddef.h>
#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    int value;
    struct Node *next;
} Node;
typedef struct { Node *head; Node *tail; size_t size; } Queue;

/* Contract: queue is valid and queue->size < SIZE_MAX. */
static int queue_enqueue(Queue *queue, int value)
{
    Node *node = malloc(sizeof *node);
    if (node == NULL) {
        return 0;
    }
    node->value = value;
    node->next = NULL;
    if (queue->head == NULL) {
        queue->head = node;
    } else {
        queue->tail->next = node;
    }
    queue->tail = node;
    ++queue->size;
    return 1;
}

/* Precondition: queue is non-NULL and points to a live writable Queue
   satisfying its ownership and chain invariant. An empty queue is allowed. */
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
    queue->head = NULL;
    queue->tail = NULL;
    queue->size = 0;
}

int main(void)
{
    Queue queue = {NULL, NULL, 0};
    int first = queue_enqueue(&queue, 10);
    int second = first && queue_enqueue(&queue, 20);

    if (!second) {
        queue_destroy(&queue);
        return 1;
    }
    printf("size=%zu first=%d last=%d\n",
           queue.size, queue.head->value, queue.tail->value);
    queue_destroy(&queue);
    printf("empty=%d size=%zu\n",
           queue.head == NULL && queue.tail == NULL, queue.size);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o queue_enqueue
./queue_enqueue
```
## 6. 코드 해석
성공 실행은 `size=2 first=10 last=20`, 다음 줄 `empty=1 size=0`을 출력한다. 실제 malloc 실패 시에는 출력 대신 이미 소유한 node를 정리하고 실패 상태로 종료한다.
## 7. 내부 동작
**[자료구조]** 처음에는 head와 tail을 같은 node로 설정한다. 다음부터는 기존 tail의 next에 연결하고 tail을 옮겨 chain 길이를 1 늘린다. tail을 보관하므로 `enqueue`의 링크 조작은 O(1)이며 순회하지 않는다. 이는 시간의 점근적 operation count이지 wall-clock time이 아니다.

**[C17 구현]** `malloc(sizeof *node)`의 NULL을 검사한 뒤에만 쓰고, `free` 전에 next를 저장한다. `SIZE_MAX` 계약은 `size_t`의 unsigned wrap으로 size 불변식이 깨지는 경우를 배제한다.

**[allocator 구현 / OS / CPU]** allocation 지연, OS 메모리 배치, CPU instruction 수는 C17이나 이 O(1) 경계에서 정하지 않는다.
## 8. 자주 하는 실수
- 첫 node를 넣을 때 tail만 설정하고 head를 놓친다.
- `tail->next`를 NULL로 끝내지 않거나 malloc 결과를 확인하지 않는다.
- 현재 size가 표현 범위를 벗어나도 증가시켜도 된다고 가정한다.
- O(1)을 일정한 실행 시간이나 instruction 수로 해석한다.
## 9. 필수 실습
빈 상태에서 한 번, 여러 번 enqueue하고 head부터의 순서와 tail, size를 확인한 뒤 전부 해제한다.
[30-11 exercise](../../exercises/30-data-structures/30-11/README.md)
## 10. 추가 실습
- ★ 단일 node에서 head와 tail이 같은지 확인한다.
- ★★ 여러 node를 순회해 size와 chain 길이를 비교한다.
- ★★★ 실패를 주입해 기존 node가 바뀌지 않음을 검증한다.
## 11. 확인 문제
1. 빈 queue의 세 필드는 각각 무엇인가?
2. 첫 삽입과 이후 삽입에서 다른 링크는 무엇인가?
3. tail이 O(1) 삽입을 가능하게 하는 이유는?
4. 실패했을 때 왜 queue에 새 링크를 남기면 안 되는가?
5. `size < SIZE_MAX` 계약은 무엇을 보장하는가?
## 12. 핵심 정리
- head와 tail을 함께 유지해야 FIFO chain과 빈 상태가 일관된다.
- 할당에 성공한 다음에만 링크와 size를 갱신한다.
- O(1)은 조작 횟수의 상한이지 실행 시간 측정치가 아니다.
## 13. 다음 Step
[30-12. `dequeue`와 마지막 node](30-12-dequeue-and-last-node.md)
## 14. 참고 자료
- N1570 6.2.5, 7.22.3, 7.22.3.3. N1570은 C11 공개 Committee Draft이며 여기서 사용한 규칙은 C17에서도 유지된다.
- Open Data Structures, Pat Morin, §3.2 `SLList` (queue operations): https://opendatastructures.org/ods-cpp/3_2_SLList_Singly_Linked_Li.html
