# 30-1 실습: 자기 참조 구조체 `Node`
이론: [note](../../../notes/30-data-structures/30-1-self-referential-node.md)
## 실습 목적
자기 참조 포인터로 NULL 종료·비순환 목록을 표현한다.
## 작성할 파일
- `main.c`
## 해야 할 일
`typedef struct Node { int value; struct Node *next; } Node;`를 선언하고 자동 저장 기간 node들을 연결한다. 빈 `head`, 한 node, 여러 node를 확인하고 각 node의 `next`를 따라 끝에서 멈춘다. 아직 동적 할당은 필요 없다.
## 사용할 개념
미완성 구조체 타입의 포인터, 선언 후 `Node`·`sizeof(Node)`, `NULL`, 객체 lifetime. 자료구조의 연결, ADT의 연산 계약, C17 표현을 따로 설명한다.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o node
```
## 실행 방법
```sh
./node
```
## 예상 관찰 결과
빈 목록에서는 읽을 node가 없고, 한 node에서는 첫 값 다음이 `NULL`이다. 여러 node는 첫·중간·마지막 값을 순서대로 보여 주고 마지막 뒤에서 멈춘다.
## 확인 포인트
`struct Node *`는 본문에서 가능하지만 `Node`와 `sizeof(Node)`는 선언 완료 후 사용한다. 포인터 크기나 allocator의 배치를 가정하지 않는다. 없는 값을 찾는 경우도 빈·비어 있지 않은 목록 모두에서 끝까지 확인해 미발견으로 처리한다.
## 추가 실습
- ★ 한 node의 첫 값과 끝을 확인한다.
- ★★ 세 node에서 첫·중간·마지막을 찾고 없는 값은 미발견으로 표시한다.
- ★★★ 빈·한 node·여러 node에서 연결을 손으로 그려 순환이 없음을 설명한다.
## 완료 기준
빈·단일·다중 및 첫·중간·마지막·미발견 경계에서 잘못된 역참조 없이 종료하고 strict build가 통과한다.
