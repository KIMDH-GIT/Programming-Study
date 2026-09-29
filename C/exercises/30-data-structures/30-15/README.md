# 30-15 실습: Part 30 종합 복습
이론: [note](../../../notes/30-data-structures/30-15-part-30-review.md)
## 실습 목적
Node/list/linked stack/linked queue의 순서·소유권·failure·cleanup을 통합한다.
## 작성할 파일
- `main.c` (학습자 작업 파일)
## 해야 할 일
node 생성과 list 순회/검색/삽입/삭제/출력/해제의 경계 사례를 표로 복습한다. linked stack과 queue를 각각 구현해 LIFO/FIFO 차이, 할당 실패 시 보존, 마지막 제거 및 종료 시 해제를 검사한다. Stack의 `size`는 `top`에서 도달 가능한 live node 수와 같고, non-NULL stack 인자는 모든 연산 진입 전에 이 불변식을 만족한다. pop의 non-NULL output은 호출과 반환까지 lifetime이 유지되는 writable `int` 객체를 가리키며 `Stack` 객체나 stack이 소유한 어떤 node와도 겹치지 않는다. dequeue output에도 같은 lifetime·writability 조건이 적용되며 `Queue` 객체나 queue가 소유한 어떤 node와도 겹치지 않는다. 각 정리 함수의 인자는 NULL이 아니며 ownership과 chain 불변식을 만족하는 살아 있는 writable structure를 가리켜야 하고, 유효한 빈 structure는 허용한다.
## 사용할 개념
Node, head/top/tail, `size_t`, `malloc`, `free`, status+output, invariant.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o part30_review
```
## 실행 방법
```sh
./part30_review
```
## 예상 관찰 결과
note의 성공 예제:
```text
stack=20 queue=10
empty=1
```

| scenario | 기대 결과 |
|---|---|
| empty list/stack/queue | 순회 없음, 제거 실패, 출력값 보존 |
| single list/stack/queue | 삭제/제거 뒤 끝점 NULL, size 0 |
| multiple: 10,20 | stack 첫 제거 20, queue 첫 제거 10 |
| allocation failure (injected) | 기존 구조의 순서/포인터/size 불변 |
| destroy and reuse | 모든 node 해제, 초기 상태에서 재사용 가능 |
## 확인 포인트
`n`은 현재 저장한 원소 수다. list 전역 순회/해제 O(n)과 끝점 연산 O(1)은 wall-clock 시간이나 CPU instruction 수가 아니다. 실패 경로에 남은 node가 없도록 한다.
## 추가 실습
- ★ 세 구조의 empty/single 전이를 비교한다.
- ★★ 두 값의 LIFO/FIFO 결과를 확인한다.
- ★★★ 실패 주입 후 정리·재사용과 leak 검사 결과를 기록한다.
## 완료 기준
각 구조의 불변식, 할당 실패, 출력 순서, cleanup을 strict build와 결정적 사례로 확인한다.
