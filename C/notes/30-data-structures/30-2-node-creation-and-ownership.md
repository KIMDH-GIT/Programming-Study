# 30-2. node 생성과 소유권
## 1. 학습 목표
- 할당 실패를 확인한 뒤 node를 초기화한다.
- 소유자·lifetime·해제 책임을 분명히 한다.
- 얕은 포인터·구조체 복사의 위험을 설명한다.
## 2. 선수 지식
30-1의 `Node` 선언, `malloc`, `free`, 포인터를 안다.
## 3. 핵심 개념
**자료구조**는 `next`로 이어진 node들이고 **ADT**는 생성·해제의 성공/실패 계약을 정의한다. **C17 표현**에서 `malloc`이 반환한 저장 공간은 성공하면 `Node` 객체로 사용하고 소유자는 결국 그 공간을 정확히 한 번 `free`할 책임을 진다. 여기서는 반환된 node를 받은 caller가 소유한다. 실패하면 `NULL`을 반환하며 기존 연결은 변경하지 않는다.

`Node` 구조체 값을 복사하면 `next` 포인터 값만 복사한다. 포인터 변수만 복사해도 소유권이 자동 분할되지 않는다. 두 복사본을 독립적인 소유자로 보고 각각 해제하면 중복 해제나 댕글링 참조가 발생한다.
## 4. 문법
```c
Node *node = malloc(sizeof *node);
if (node == NULL) {
    return NULL;
}
node->value = value;
node->next = NULL;
```
`struct Node *`는 구조체 본문에서 미완성 타입의 포인터로 쓸 수 있고 typedef `Node` 및 `sizeof(Node)`는 선언이 완료된 후 쓴다. 크기·주소 간격·배치 방식은 가정하지 않는다.
## 5. 최소 코드 예제
```c
#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    int value;
    struct Node *next;
} Node;

static Node *create_node(int value)
{
    Node *node = malloc(sizeof *node);

    if (node == NULL) {
        return NULL;
    }
    node->value = value;
    node->next = NULL;
    return node;
}

int main(void)
{
    Node *head = create_node(42);

    if (head == NULL) {
        puts("allocation failed");
        return 1;
    }
    printf("value=%d end=%d\n", head->value, head->next == NULL);
    free(head);
    head = NULL;
    return 0;
}
```
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o create_node
./create_node
```
## 6. 코드 해석
정상 할당에서는 `value=42 end=1`을 출력한다. 할당 실패 시에는 `allocation failed`를 출력하고 1로 종료한다. 성공 시 caller인 `main`이 소유하고 해제한 뒤 저장해 둔 포인터를 `NULL`로 만든다.
## 7. 내부 동작
**[자료구조]** 새 node의 `next == NULL`은 단일 node 목록의 끝이다. 이 Part의 목록은 비순환이며 sentinel node가 없다.

**[ADT]** 생성 실패는 node를 반환하지 않고 기존 구조를 바꾸지 않는다. 소유권은 C 문법이 아니라 호출자와 함수 사이의 계약이다.

**[C17]** 할당이 성공하면 사용 가능한 저장 공간이 반환되고 `free` 이후에는 해당 객체에 접근할 수 없다. 주소 크기나 allocator가 node를 연속으로 배치하는지 여부는 보장하지 않는다.
## 8. 자주 하는 실수
- `malloc` 반환값 검사를 건너뛰고 멤버에 접근한다.
- `Node` 또는 포인터 복사만으로 독립적인 할당을 했다고 생각한다.
- `free` 뒤 저장된 별칭을 역참조하거나 같은 할당을 다시 해제한다.
## 9. 필수 실습
성공과 실패에서 소유자·해제 책임을 명시한다. [30-2 exercise](../../exercises/30-data-structures/30-2/README.md)
## 10. 추가 실습
- ★ 단일 node 생성·해제를 확인한다.
- ★★ 두 node를 연결한 뒤 각각 정확히 한 번 해제한다.
- ★★★ 구조체 복사가 `next` 대상의 소유권까지 복제하지 않는 이유를 설명한다.
## 11. 확인 문제
1. `malloc(sizeof *node)`가 타입 크기를 구하는 시점은?
2. 실패 시 기존 연결은 어떻게 유지하는가?
3. 반환된 node를 누가 해제하는가?
4. 포인터 복사가 깊은 복사가 아닌 이유는?
5. `free` 이후 접근이 잘못된 이유는?
## 12. 핵심 정리
- 할당 성공을 확인한 후 초기화·연결한다.
- 소유자는 lifetime이 끝날 때 정확히 한 번 해제한다.
## 13. 다음 Step
[30-3. linked list 순회·검색](30-3-linked-list-traversal-and-search.md)
## 14. 참고 자료
- N1570 6.2.4, 7.22.3. N1570은 **C11 공개 Committee Draft**이며 관련 lifetime·할당 규칙은 C17에서도 유지된다.
- [cppreference: malloc](https://en.cppreference.com/w/c/memory/malloc), [free](https://en.cppreference.com/w/c/memory/free)
