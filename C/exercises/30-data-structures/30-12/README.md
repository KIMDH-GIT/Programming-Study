# 30-12 실습: `dequeue`와 마지막 node
이론: [note](../../../notes/30-data-structures/30-12-dequeue-and-last-node.md)
## 실습 목적
FIFO 제거와 빈 상태의 세 필드 동시 복원을 연습한다.
## 작성할 파일
- `main.c` (학습자 작업 파일)
## 해야 할 일
`queue_dequeue(Queue *queue, int *value)`를 구현한다. NULL 또는 빈 상태에서 0을 돌리고 출력 인자는 그대로 둔다. non-NULL output은 호출과 반환까지 lifetime이 유지되는 writable `int` 객체를 가리키며 `Queue` 객체나 queue가 소유한 어떤 node와도 겹치지 않는다. 성공 시 값/next를 저장하고 head와 size를 갱신하고, 마지막 node면 tail을 NULL로 만든 뒤 해제한다. 정리 함수의 인자는 NULL이 아니며 ownership과 chain 불변식을 만족하는 살아 있는 writable `Queue`를 가리켜야 하고, 유효한 빈 queue는 허용한다.
## 사용할 개념
linked queue, status+output, lifetime, NULL-terminated chain.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o queue_dequeue
```
## 실행 방법
```sh
./queue_dequeue
```
## 예상 관찰 결과
note의 성공 실행 출력:
```text
empty=0 value=77
single=-1 empty=1
out=10
out=20
reuse=30 size=0
```

| scenario | 기대 결과 |
|---|---|
| empty 또는 NULL 인자 | status 0, 출력 인자 불변 |
| single (-1) -> empty | status 1, -1 출력, head/tail NULL, size 0 |
| multiple (10,20) | 10 다음 20, 매번 size 감소 |
| dequeue all | 마지막 뒤 빈 상태 유지 |
| enqueue allocation failure | status 0, 이전 chain/order 불변 |
| enqueue again after empty | 새 head == tail; 정상 dequeue |
## 확인 포인트
free 이후 old를 역참조하지 않는다. -1은 정상 값이며 실패 코드가 아니다.
## 추가 실습
- ★ NULL output 인자를 검사한다.
- ★★ 세 node를 모두 꺼내 size 변화를 기록한다.
- ★★★ 실패 주입 후 남은 node를 전부 꺼내고 재사용한다.
## 완료 기준
빈/단일/여러 node/실패/재사용의 상태와 출력이 명세와 일치한다.
