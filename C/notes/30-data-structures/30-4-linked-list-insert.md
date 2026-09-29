# 30-4. linked list insert
## 1. 학습 목표
- 빈 목록과 기존 목록의 앞에 node를 삽입한다.
- `Node **head`가 caller의 첫 포인터를 갱신하는 이유를 설명한다.
- 할당 실패 시 연결을 보존한다.
## 2. 선수 지식
30-2의 할당·소유권과 30-3의 연결 순회를 안다.
## 3. 핵심 개념
**자료구조**는 NULL 종료·비순환 node 연결이며 sentinel은 없다. **ADT**의 삽입은 성공 시 새 값을 지정 위치에 연결하고 실패 시 기존 순서를 보존한다. **C17 표현**에서는 첫 포인터를 변경하려면 그 포인터 객체의 주소 `Node **head`를 전달한다. `Node *head`를 값으로 받으면 지역 복사본만 바뀐다. 중간·끝 삽입은 유효한 이전 node를 찾은 뒤 새 node의 `next`를 이전의 `next`에 연결하고 이전의 `next`를 새 node로 바꾼다.
## 4. 문법
```c
static int insert_front(Node **head, int value);
```
caller는 유효한 writable `Node *` 객체의 주소를 전달한다. `struct Node *next`는 구조체 본문에서 미완성 타입 포인터로 유효하며 typedef `Node`, `sizeof(Node)`는 선언 완료 후 사용한다.
## 5. 최소 코드 예제
```c
#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    int value;
    struct Node *next;
} Node;

static int insert_front(Node **head, int value)
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

int main(void)
{
    Node *head = NULL;

    if (!insert_front(&head, 20)) {
        puts("allocation failed");
        return 1;
    }
    if (!insert_front(&head, 10)) {
        free(head);
        puts("allocation failed");
        return 1;
    }
    printf("first=%d last=%d end=%d\n",
           head->value, head->next->value, head->next->next == NULL);
    Node *second = head->next;
    free(head);
    free(second);
    return 0;
}
```
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o insert
./insert
```
## 6. 코드 해석
정상 할당에서 `first=10 last=20 end=1`이다. 빈 목록에서 20을 넣고 앞에 10을 넣는다. 실패하면 그 호출은 `*head`를 변경하지 않는다. caller는 성공한 두 할당을 각각 한 번 해제한다.
## 7. 내부 동작
**[자료구조]** 새 node의 `next`를 옛 첫 node에 설정한 뒤에만 `*head`를 바꿔 기존 목록을 잃지 않는다.

**[ADT]** 위치를 이미 알고 있을 때 링크 변경은 `O(1)`이다. 위치를 순회해 찾고 삽입하면 현재 node 수 `n`에 대해 `O(n)`이다.

**[C17]** 성공한 할당에서만 멤버를 기록한다. 새 node의 소유권은 성공 후 caller의 목록으로 이전하고 나중에 해제해야 한다. 포인터 크기나 객체의 메모리 배치는 전제하지 않는다.
## 8. 자주 하는 실수
- `Node *head`만 받아 caller의 `head`가 바뀐 것으로 생각한다.
- 새 node의 `next`를 저장하기 전 이전 링크를 덮어쓴다.
- 할당 실패를 확인하기 전에 `*head`를 바꾼다.
## 9. 필수 실습
빈·단일·다중 목록에 앞·중간·끝을 삽입한다. [30-4 exercise](../../exercises/30-data-structures/30-4/README.md)
## 10. 추가 실습
- ★ 빈 목록과 단일 목록에 앞 삽입을 구현한다.
- ★★ 알려진 이전 node 다음에 삽입한다.
- ★★★ 검색 후 삽입과 실패 시 연결 보존을 검증한다.
## 11. 확인 문제
1. `Node *head`를 값으로 받으면 caller에서 무엇이 바뀌는가?
2. 새 node 연결에서 두 링크 변경의 순서는?
3. 할당 실패 직후 `*head` 값은?
4. 이미 위치를 아는 삽입과 위치 검색을 포함한 삽입의 시간은?
5. 삽입 성공 후 node를 누가 해제하는가?
## 12. 핵심 정리
- 새 공간을 확보한 후 링크를 잇고 caller의 첫 포인터를 갱신한다.
- 위치를 찾는 비용과 링크 변경 비용을 구분한다.
## 13. 다음 Step
[30-5. linked list delete와 head 갱신](30-5-linked-list-delete-and-head-update.md)
## 14. 참고 자료
- N1570 6.5.3.2, 7.22.3. N1570은 **C11 공개 Committee Draft**이며 관련 포인터·할당 규칙은 C17에서도 유지된다.
- [cppreference: malloc](https://en.cppreference.com/w/c/memory/malloc), [free](https://en.cppreference.com/w/c/memory/free)
