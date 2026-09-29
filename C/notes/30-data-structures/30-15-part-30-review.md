# 30-15. Part 30 종합 복습
## 1. 학습 목표
- Node/list/linked stack/linked queue의 ownership과 불변식을 함께 복습한다.
- allocation 실패와 node 해제를 모든 변경 경로에서 점검한다.
- ADT, C17, 도구, OS, CPU 층을 구분한다.
## 2. 선수 지식
30-1부터 30-14까지의 node 생성, list 조작/정리, linked stack과 linked queue를 안다.
## 3. 핵심 개념
**[자료구조]** `Node`의 next로 만든 acyclic/NULL-terminated chain은 list와 linked stack/queue의 기반이다. queue는 empty에서 head/tail NULL, size 0; nonempty에서 head/tail non-NULL, tail->next NULL, size == chain 길이다. linked stack은 top에서 최근 삽입 node를 먼저 제거하고 `size == 0` iff `top == NULL`이며, `size`는 `top`에서 도달 가능한 live node 수와 같다. non-NULL stack 인자는 모든 연산 진입 전에 이 불변식을 만족해야 한다.

**[ADT / operation]** stack push/pop은 LIFO, queue enqueue/dequeue는 FIFO다. stack pop의 non-NULL output은 호출과 반환까지 lifetime이 유지되는 writable `int` 객체를 가리키며 `Stack` 객체나 stack이 소유한 어떤 node와도 겹치지 않는다. queue dequeue의 non-NULL output에도 같은 lifetime·writability 조건이 적용되며 `Queue` 객체나 queue가 소유한 어떤 node와도 겹치지 않는다. NULL allocation 결과는 기존 구조를 보존해야 한다. `n`은 각각의 구조가 현재 저장한 원소 수다.
## 4. 문법
```c
typedef struct Node { int value; struct Node *next; } Node;
typedef struct { Node *head; Node *tail; size_t size; } Queue;
typedef struct { Node *top; size_t size; } Stack;
```
## 5. 최소 코드 예제
```c
#include <stdint.h>
#include <stddef.h>
#include <stdio.h>
#include <stdlib.h>

typedef struct Node { int value; struct Node *next; } Node;
typedef struct { Node *head; Node *tail; size_t size; } Queue;
typedef struct { Node *top; size_t size; } Stack;

/* Contract: valid structure and size < SIZE_MAX. */
static int stack_push(Stack *stack, int value)
{
    Node *node = malloc(sizeof *node);
    if (node == NULL) return 0;
    node->value = value;
    node->next = stack->top;
    stack->top = node;
    ++stack->size;
    return 1;
}

/* A non-NULL output must be writable, live through return, and overlap
   neither the Stack object nor any node it owns. */
static int stack_pop(Stack *stack, int *value)
{
    if (stack == NULL || value == NULL || stack->top == NULL) return 0;
    Node *old = stack->top;
    int removed = old->value;
    stack->top = old->next;
    --stack->size;
    *value = removed;
    free(old);
    return 1;
}

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

/* A non-NULL output must be writable, live through return, and overlap
   neither the Queue object nor any node it owns. */
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

/* Precondition: stack is non-NULL and points to a live writable Stack
   satisfying its ownership and chain invariant. An empty stack is allowed. */
static void stack_destroy(Stack *stack)
{
    Node *node = stack->top;
    while (node != NULL) {
        Node *next = node->next;
        free(node);
        node = next;
    }
    stack->top = NULL;
    stack->size = 0;
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
    Stack stack = {NULL, 0};
    Queue queue = {NULL, NULL, 0};
    int from_stack = 0;
    int from_queue = 0;
    if (!stack_push(&stack, 10) || !stack_push(&stack, 20) ||
        !queue_enqueue(&queue, 10) || !queue_enqueue(&queue, 20)) {
        stack_destroy(&stack);
        queue_destroy(&queue);
        return 1;
    }
    if (!stack_pop(&stack, &from_stack) ||
        !queue_dequeue(&queue, &from_queue)) {
        stack_destroy(&stack);
        queue_destroy(&queue);
        return 1;
    }
    printf("stack=%d queue=%d\n", from_stack, from_queue);
    stack_destroy(&stack);
    queue_destroy(&queue);
    printf("empty=%d\n", stack.top == NULL && stack.size == 0 &&
           queue.head == NULL && queue.tail == NULL && queue.size == 0);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o part30_review
./part30_review
```
## 6. 코드 해석
같은 10,20을 넣어도 pop은 20, dequeue는 10을 출력한다: `stack=20 queue=10`, 다음 줄 `empty=1`. 실패가 발생하면 양쪽 구조의 성공한 allocation을 모두 해제한 뒤 1로 종료한다.
## 7. 내부 동작
**[자료구조]** list의 삽입/삭제 시 head 연결과 소유권을 추적한다. stack은 top 한 곳을, queue는 head와 tail을 유지한다.

**[ADT / operation]** 각 끝점의 push/pop/enqueue/dequeue는 O(1), 전체 chain 순회와 destroy는 O(n)이다. Big-O는 operation count의 성장률이지 wall-clock time이 아니다.

**[C17 구현]** malloc NULL 확인, `size < SIZE_MAX` 계약, 해제 전 next/값 저장, 해제 후 접근 금지를 각 구조에 적용한다.

**[allocator 구현 / OS / CPU]** 할당 시간과 실제 메모리 반환은 구현/OS에 의존한다. CPU instruction 수는 C operation count와 같지 않다. 도구의 clean 결과는 관찰한 경로의 evidence이며 일반적인 correctness proof는 아니다.

**[MIPS — 수업 기준]** node 조작 횟수는 MIPS instruction 수와 다르다.

**[RISC-V — 병행 학습]** RISC-V의 load/store·branch·ABI는 C 자료구조 불변식을 정의하지 않는다.
## 8. 자주 하는 실수
- list의 head 제거, queue의 마지막 제거에서 끝점 포인터를 갱신하지 않는다.
- 실패 후 이미 할당된 다른 구조의 node를 잊는다.
- `free` 후 next를 읽거나 한 node를 두 번 해제한다.
- sanitizer 통과를 모든 입력에서의 보증으로 부른다.
## 9. 필수 실습
Node/list의 순회·검색·삽입·삭제·출력·해제와 linked stack/queue의 empty, single, multiple, failure, reuse 사례를 표로 정리한다.
[30-15 exercise](../../exercises/30-data-structures/30-15/README.md)
## 10. 추가 실습
- ★ LIFO와 FIFO의 출력 차이를 기록한다.
- ★★ 단일 node 제거 뒤 모든 끝점 불변식을 비교한다.
- ★★★ 실패 주입과 leak 도구 관찰을 각각 기록한다.
## 11. 확인 문제
1. list와 linked stack, linked queue가 공유하는 node 소유권 규칙은?
2. 마지막 queue node를 제거하면 어떤 세 필드가 바뀌는가?
3. stack과 queue에서 10,20을 넣은 뒤 첫 제거값은 각각?
4. 실패한 allocation 후 어떤 상태를 보존해야 하는가?
5. destroy의 O(n)에서 n은 무엇인가?
6. C operation count와 ISA instruction count는 왜 다른가?
7. leak 도구 실행만으로 일반적 정확성을 증명할 수 없는 이유는?
## 12. 핵심 정리
- linked structure의 포인터, chain 길이, ownership을 함께 검토한다.
- 실패와 마지막 node 제거는 구조 보존의 핵심 경계다.
- 코드의 C17 규칙과 allocator/OS/CPU 관찰은 구별한다.
## 13. 다음 Step
31-1. C abstract machine과 observable behavior
## 14. 참고 자료
- N1570 6.2.4, 7.22.3. N1570은 C11 공개 Committee Draft이며 사용한 lifetime/allocation 규칙은 C17에서도 유지된다.
- Open Data Structures, Pat Morin, §3.2 `SLList`: https://opendatastructures.org/ods-cpp/3_2_SLList_Singly_Linked_Li.html
- GCC, Instrumentation Options: https://gcc.gnu.org/onlinedocs/gcc/Instrumentation-Options.html
