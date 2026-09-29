# 30-14. 메모리 누수 검증
## 1. 학습 목표
- queue에 남은 node를 정상 경로와 실패 경로에서 전부 해제한다.
- leak, dangling pointer, use-after-free, double-free를 구분한다.
- sanitizer와 Valgrind 관찰의 범위를 설명한다.
## 2. 선수 지식
30-11부터 30-13까지의 linked queue 불변식과 실패 경로를 안다.
## 3. 핵심 개념
**[C17 구현]** 할당한 node를 더는 쓰지 않으면서 해제하지 않으면 leak이다. dangling pointer는 lifetime이 끝난 객체를 여전히 가리키는 pointer이고, 그것을 역참조하면 use-after-free (undefined behavior)다. 같은 객체를 다시 `free`하면 double-free (undefined behavior)다. 단순히 포인터를 NULL로 만드는 것만으로 다른 별칭까지 안전해지지 않는다.

**[자료구조]** empty의 head/tail/size는 NULL/NULL/0, nonempty는 acyclic/NULL-terminated chain, head/tail non-NULL, tail->next NULL이며 size는 chain 길이다. `n`은 현재 저장한 원소 수다.

**[ADT / output contract]** `queue_dequeue`의 non-NULL output은 호출과 반환까지 lifetime이 유지되는 writable `int` 객체를 가리키며 `Queue` 객체나 queue가 소유한 어떤 node와도 겹치지 않아야 한다. NULL output은 상태와 output을 변경하지 않는 실패다.

**[ADT / output contract]** `queue_dequeue`의 non-NULL output은 호출과 반환까지 lifetime이 유지되는 writable `int` 객체를 가리키며 `Queue` 객체나 queue가 소유한 어떤 node와도 겹치지 않아야 한다. NULL output은 상태와 output을 변경하지 않는 실패다.
## 4. 문법
```c
Node *next = node->next; /* free 이전에 저장 */
free(node);
node = next;
/* 마지막에 head/tail/size를 NULL/NULL/0으로 설정 */
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
    int value = 0;
    if (!queue_enqueue(&queue, 10) || !queue_enqueue(&queue, 20)) {
        queue_destroy(&queue);
        return 1;
    }
    if (!queue_dequeue(&queue, &value)) {
        queue_destroy(&queue);
        return 1;
    }
    printf("out=%d remaining=%zu\n", value, queue.size);
    queue_destroy(&queue);
    printf("empty=%d\n",
           queue.head == NULL && queue.tail == NULL && queue.size == 0);
    if (!queue_enqueue(&queue, 30)) return 1;
    queue_destroy(&queue);
    printf("reuse_empty=%d\n",
           queue.head == NULL && queue.tail == NULL && queue.size == 0);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o queue_cleanup
./queue_cleanup
```

별도 도구 관찰 (지원하는 GCC/환경에서만):
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -g -O1 \
    -fsanitize=address,leak -fno-omit-frame-pointer main.c -o queue_san
ASAN_OPTIONS=detect_leaks=1 ./queue_san
valgrind --leak-check=full --show-leak-kinds=all ./queue_cleanup
```
## 6. 코드 해석
성공 실행 출력은 `out=10 remaining=1`, `empty=1`, `reuse_empty=1`이다. dequeue가 첫 node를 해제하고 destroy가 남은 node를 해제한다. 두 번째 destroy는 재사용 후 node도 해제한다.
## 7. 내부 동작
**[ADT / operation]** `enqueue`는 O(1), `dequeue`는 O(1), `destroy`는 O(n)의 node 방문이다. 이는 wall-clock이 아니라 방문/링크 조작 횟수다.

**[C17 구현]** 실패한 두 번째 allocation에서도 이미 할당한 첫 node를 정리한다. `free` 후 old/node를 읽지 않으며, destroy는 모든 연결을 따라가고 empty로 초기화한다.

**[allocator 구현 / OS / CPU]** ASan/LeakSanitizer/Valgrind는 구현 환경의 검증 도구이지 C17 보장이나 모든 입력에서의 정확성 증명이 아니다. 실제 메모리 반환 시각과 CPU instruction 수는 도구 결과만으로 결정되지 않는다.
## 8. 자주 하는 실수
- dequeue한 node만 해제하고 queue에 남은 node를 잊는다.
- 해제한 node를 가리키는 tail을 그대로 둔다.
- free 후 `node->next`를 읽는다.
- report가 없다는 사실을 모든 경로의 누수 부재 증명이라고 부른다.
## 9. 필수 실습
빈, 한 node, 여러 node, 부분 allocation 실패, dequeue 후 잔여 node, 재사용 뒤 종료에서 모든 node가 해제되는지 확인한다.
[30-14 exercise](../../exercises/30-data-structures/30-14/README.md)
## 10. 추가 실습
- ★ 빈 queue에 destroy를 호출한다.
- ★★ 일부 dequeue 후 남은 chain을 destroy한다.
- ★★★ 지원 환경에서 LeakSanitizer와 Valgrind를 실행하고 결과를 비교한다.
## 11. 확인 문제
1. leak과 dangling pointer의 차이는 무엇인가?
2. dangling pointer를 역참조하면 어떤 문제가 생기는가?
3. double-free는 어떤 동작인가?
4. destroy 전에 next를 저장하는 이유는?
5. sanitizer 결과가 correctness proof가 아닌 이유는?
## 12. 핵심 정리
- 소유한 모든 node를 성공·실패·재사용 종료 경로에서 해제한다.
- `free` 이후에는 해제한 node의 필드를 읽지 않는다.
- 도구의 clean 실행은 특정 실행에 관한 evidence다.
## 13. 다음 Step
[30-15. Part 30 종합 복습](30-15-part-30-review.md)
## 14. 참고 자료
- N1570 6.2.4, 7.22.3.3, 7.22.3.4. N1570은 C11 공개 Committee Draft이며 사용한 lifetime/allocation 규칙은 C17에서도 유지된다.
- GCC, Instrumentation Options (`-fsanitize=address`, `-fsanitize=leak`): https://gcc.gnu.org/onlinedocs/gcc/Instrumentation-Options.html
- cppreference, `free`: https://en.cppreference.com/w/c/memory/free
