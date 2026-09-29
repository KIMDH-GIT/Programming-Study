# 30-13. allocation 실패와 구조 보존
## 1. 학습 목표
- allocation 실패가 queue의 기존 상태를 바꾸지 않도록 한다.
- 실패를 주입해 head/tail/size/order 보존을 결정적으로 관찰한다.
- 실패 후 기존 node의 사용과 재시도를 검증한다.
## 2. 선수 지식
30-11의 `enqueue`와 30-12의 `dequeue`, node 소유권을 안다.
## 3. 핵심 개념
**[ADT / operation]** `enqueue` 실패는 0을 돌려주고 기존 원소 순서 및 head/tail/size를 보존한다. **[자료구조]** 빈 상태는 세 필드가 NULL/NULL/0이고, 비어 있지 않으면 acyclic/NULL-terminated chain, tail->next NULL, size == chain 길이다.

**[allocator 구현]** C17에서 `malloc` 실패를 이 프로그램에서 포터블하게 강제할 방법은 없다. 아래 예제는 실제 malloc을 쓰는 기본 경로와 테스트용 NULL 반환 경로를 한 작은 allocator 함수에 둔다. 테스트용 `fail_next`는 queue 포인터를 건드리지 않는다. `n`은 현재 저장한 원소 수다.
## 4. 문법
```c
typedef struct { Node *head; Node *tail; size_t size; } Queue;
/* 호출 전 valid queue, size < SIZE_MAX. */
static int queue_enqueue(Queue *queue, int value);
```
## 5. 최소 코드 예제
```c
#include <stdint.h>
#include <stddef.h>
#include <stdio.h>
#include <stdlib.h>

typedef struct Node { int value; struct Node *next; } Node;
typedef struct { Node *head; Node *tail; size_t size; } Queue;
static int fail_next;

static Node *allocate_node(void)
{
    if (fail_next) {
        fail_next = 0;
        return NULL;
    }
    Node *node = malloc(sizeof *node);
    return node;
}

/* Contract: valid queue and size < SIZE_MAX. */
static int queue_enqueue(Queue *queue, int value)
{
    Node *node = allocate_node();
    if (node == NULL) return 0;
    node->value = value;
    node->next = NULL;
    if (queue->head == NULL) queue->head = node;
    else queue->tail->next = node;
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
    queue->head = queue->tail = NULL;
    queue->size = 0;
}

int main(void)
{
    Queue queue = {NULL, NULL, 0};
    fail_next = 1;
    int empty_failure = !queue_enqueue(&queue, 1);
    int empty_unchanged = queue.head == NULL && queue.tail == NULL &&
                          queue.size == 0;
    if (!queue_enqueue(&queue, 10) || !queue_enqueue(&queue, 20)) {
        queue_destroy(&queue);
        return 1;
    }
    Node *before_head = queue.head;
    Node *before_tail = queue.tail;
    size_t before_size = queue.size;
    fail_next = 1;
    int failed = !queue_enqueue(&queue, 30);
    int preserved = queue.head == before_head && queue.tail == before_tail &&
                    queue.size == before_size && queue.head->value == 10 &&
                    queue.head->next == queue.tail && queue.tail->value == 20 &&
                    queue.tail->next == NULL;
    if (!queue_enqueue(&queue, 30)) {
        queue_destroy(&queue);
        return 1;
    }
    printf("empty_failure=%d empty_unchanged=%d\n",
           empty_failure, empty_unchanged);
    printf("failure=%d preserved=%d\n", failed, preserved);
    printf("reuse=%d,%d,%d size=%zu\n", queue.head->value,
           queue.head->next->value, queue.tail->value, queue.size);
    queue_destroy(&queue);
    return !(empty_failure && empty_unchanged && failed && preserved);
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o queue_failure
./queue_failure
```
## 6. 코드 해석
주입한 두 실패는 각각 빈 queue와 기존 두 node가 있는 queue에서 발생한다. 출력은 `empty_failure=1 empty_unchanged=1`, `failure=1 preserved=1`, `reuse=10,20,30 size=3`이다. 실제 malloc 실패가 발생하면 부분 chain도 정리하고 1로 종료한다.
## 7. 내부 동작
**[C17 구현]** NULL인 node에 쓰지 않고, 초기화된 node만 연결한다. 크기 증가가 표현 가능하도록 호출 전 `size < SIZE_MAX`를 요구한다. allocation이 실패하면 함수가 연결 변경 전에 반환한다.

**[자료구조 / ADT]** tail 유지로 연결은 O(1); 실패 뒤에도 size와 실제 chain 길이가 일치한다.

**[allocator 구현 / OS / CPU]** 주입은 테스트 정책이며 실제 메모리 부족 또는 OS의 할당 정책을 시뮬레이션해 보장하는 것은 아니다. O(1)은 wall-clock 또는 instruction count가 아니다.
## 8. 자주 하는 실수
- malloc NULL 확인 전에 `tail->next` 또는 size를 갱신한다.
- malloc 실패가 적은 크기의 요청으로 반드시 재현된다고 주장한다.
- 실패 때 tail은 그대로 두고 size만 증가시킨다.
- 성공 node를 실패 검증 중 잃어버려 leak을 만든다.
## 9. 필수 실습
빈 상태 및 복수 node 상태에서 실패를 각각 주입하고 pointer identity, size, 순서를 비교한다.
[30-13 exercise](../../exercises/30-data-structures/30-13/README.md)
## 10. 추가 실습
- ★ 실패 후 빈 queue가 그대로인지 검사한다.
- ★★ 복수 node의 head/tail 주소와 chain 값을 보관해 비교한다.
- ★★★ 실패 후 다시 성공시킨 다음 모든 node를 해제한다.
## 11. 확인 문제
1. 실패 주입 없이는 무엇을 결정적으로 재현할 수 없는가?
2. 연결을 allocation 이후로 미루는 이유는?
3. 실패 후 값뿐 아니라 pointer identity도 검사하는 이유는?
4. 실패한 노드의 소유권은 누구에게 있는가?
5. `size < SIZE_MAX` 계약을 명시하는 이유는?
## 12. 핵심 정리
- 할당 성공 전에는 queue를 변경하지 않는다.
- 실패 반환 뒤 기존 chain의 순서와 모든 필드가 보존된다.
- 테스트 주입과 OS 실제 메모리 부족은 다른 층이다.
## 13. 다음 Step
[30-14. 메모리 누수 검증](30-14-memory-leak-validation.md)
## 14. 참고 자료
- N1570 7.22.3, 7.22.3.4. N1570은 C11 공개 Committee Draft이며 사용한 allocation/lifetime 규칙은 C17에서도 유지된다.
- Open Data Structures, Pat Morin, §3.2 `SLList`: https://opendatastructures.org/ods-cpp/3_2_SLList_Singly_Linked_Li.html
