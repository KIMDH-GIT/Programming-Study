# 30-5. linked list delete와 head 갱신
## 1. 학습 목표
- 첫 일치 node를 연결에서 제거하고 해제한다.
- 첫 node 삭제 시 caller의 `head`를 갱신한다.
- 성공 status와 output으로 삭제 값을 전달한다.
## 2. 선수 지식
30-2의 소유권과 30-4의 `Node **head`를 안다.
## 3. 핵심 개념
**자료구조**는 NULL 종료·비순환 목록이며 sentinel이 없다. **ADT**의 삭제 계약은 첫 일치 항목만 제거하고 성공이면 값을 output에 기록하며, 미발견이면 0을 반환하고 output·목록을 그대로 둔다는 것이다. **C17 표현**에서는 `Node **link`가 첫 포인터 또는 이전 node의 `next`를 가리키므로 양쪽을 같은 코드로 갱신할 수 있다. `Node *head`를 값으로 받아 변경하면 caller의 첫 포인터는 갱신되지 않는다.
## 4. 문법
```c
static int delete_first(Node **head, int target, int *out);
```
`head`와 `out`은 NULL이 아니다. `head`는 owned node 밖에 있는 살아 있는 writable caller-owned `Node *` 객체를 가리키며 `*head == NULL`은 유효한 빈 목록이다. `out`은 호출과 반환까지 lifetime이 유지되는 writable `int` 객체를 가리키고, 그 객체는 `head`가 가리키는 첫 포인터 객체나 목록이 소유한 어떤 node와도 겹치지 않는다. 성공 시에만 `*out`이 변경된다. `struct Node *`는 선언 본문에서 미완성 타입의 포인터로 쓰고 typedef `Node`·`sizeof(Node)`는 완료 후 사용한다.
## 5. 최소 코드 예제
```c
#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    int value;
    struct Node *next;
} Node;

/* Precondition: head is non-NULL and points to a live writable
   caller-owned Node * object outside the owned chain; *head may be NULL. */
static int prepend(Node **head, int value)
{
    Node *node = malloc(sizeof *node);
    if (node == NULL) {
        return 0;
    }
    node->value = value;
    node->next = *head;
    *head = node;
    return 1;
}

/* Precondition: head and out are non-NULL. out points to a writable int
   whose lifetime extends through return and overlaps neither the head
   object nor any node owned by the list. */
static int delete_first(Node **head, int target, int *out)
{
    Node **link = head;
    while (*link != NULL) {
        Node *victim = *link;
        if (victim->value == target) {
            Node *next = victim->next;
            int value = victim->value;
            *link = next;
            free(victim);
            *out = value;
            return 1;
        }
        link = &victim->next;
    }
    return 0;
}

/* Precondition: head is non-NULL and points to a live writable
   caller-owned Node * object outside the owned chain; *head may be NULL. */
static void clear(Node **head)
{
    while (*head != NULL) {
        Node *next = (*head)->next;
        free(*head);
        *head = next;
    }
}

int main(void)
{
    Node *head = NULL;
    int removed = 0;

    printf("empty=%d\n", delete_first(&head, 10, &removed));
    if (!prepend(&head, 30) || !prepend(&head, 20) ||
        !prepend(&head, 10)) {
        clear(&head);
        puts("allocation failed");
        return 1;
    }
    printf("middle=%d ", delete_first(&head, 20, &removed));
    printf("value=%d\n", removed);
    printf("first=%d ", delete_first(&head, 10, &removed));
    printf("head=%d\n", head->value);
    printf("absent=%d\n", delete_first(&head, 99, &removed));
    printf("single=%d ", delete_first(&head, 30, &removed));
    printf("empty_now=%d\n", head == NULL);
    if (!prepend(&head, 40) || !prepend(&head, 50)) {
        clear(&head);
        puts("allocation failed");
        return 1;
    }
    printf("last=%d\n", delete_first(&head, 40, &removed));
    clear(&head);
    return 0;
}
```
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o delete
./delete
```
## 6. 코드 해석
정상 할당의 출력은 `empty=0`, `middle=1 value=20`, `first=1 head=30`, `absent=0`, `single=1 empty_now=1`, `last=1` 순서다. 할당 실패 시 이미 만들어진 node만 해제하고 종료한다.
## 7. 내부 동작
**[자료구조]** 현재 링크가 가리키는 node의 `next`와 값을 먼저 저장하고 링크를 우회시킨 후 해제한다. 첫 node·중간·마지막 모두 이 순서를 사용한다.

**[ADT]** 미발견이면 head와 output이 바뀌지 않는다. 첫 일치 node를 찾는 데 현재 node 수 `n` 기준 최악 `O(n)`, 위치를 안 뒤 연결 변경은 `O(1)`이다.

**[C17]** `free` 후 `victim`이나 그 멤버를 다시 읽지 않는다. 제거한 node의 다른 별칭은 더 이상 유효한 객체를 가리키지 않는다. 포인터 크기나 할당 배치는 가정하지 않는다.
## 8. 자주 하는 실수
- 첫 node를 해제한 뒤 caller의 `head`를 갱신한다.
- `free(victim)` 다음에 `victim->next`나 `victim->value`를 읽는다.
- 미발견 시 `-1`을 반환해 실제 값과 실패를 섞는다.
## 9. 필수 실습
빈·첫·중간·마지막·미발견·단일 node에서 빈 목록으로의 전환을 확인한다. [30-5 exercise](../../exercises/30-data-structures/30-5/README.md)
## 10. 추가 실습
- ★ 단일 node 삭제 후 `head == NULL`을 확인한다.
- ★★ 세 node에서 첫·중간·마지막 삭제를 각각 검증한다.
- ★★★ 중복 값에서는 첫 일치만 제거하고 나머지를 보존한다.
## 11. 확인 문제
1. 첫 node를 삭제할 때 caller의 어느 객체가 바뀌는가?
2. `free` 전에 무엇을 저장해야 하는가?
3. 미발견에서 output의 상태는?
4. `-1` 대신 status/output을 사용하는 이유는?
5. 한 node 삭제 후 목록은 어떻게 표현하는가?
## 12. 핵심 정리
- 연결을 우회시키고 난 뒤 해제하며 해제된 객체에 접근하지 않는다.
- 첫 포인터까지 갱신하려면 그 주소를 전달한다.
## 13. 다음 Step
[30-6. linked list print](30-6-linked-list-print.md)
## 14. 참고 자료
- N1570 6.2.4, 7.22.3. N1570은 **C11 공개 Committee Draft**이며 관련 lifetime·해제 규칙은 C17에서도 유지된다.
- [cppreference: free](https://en.cppreference.com/w/c/memory/free)
