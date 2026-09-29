# 30-11 실습: queue 불변식과 `enqueue`
이론: [note](../../../notes/30-data-structures/30-11-queue-invariant-and-enqueue.md)
## 실습 목적
head/tail/size 불변식을 유지하며 linked queue에 O(1)로 삽입한다.
## 작성할 파일
- `main.c` (학습자 작업 파일)
## 해야 할 일
`Queue`와 `Node`를 정의하고 `queue_enqueue` 및 모든 node를 해제하는 함수를 작성한다. `queue_enqueue`는 NULL이 아닌 유효한 `Queue`와 `size < SIZE_MAX`를 precondition으로 받는다. 빈 경우에는 head와 tail을 동시에 설정하고, 여러 node에서는 tail에 연결한다. 할당 NULL이면 상태를 그대로 둔다. 정리 함수의 인자는 NULL이 아니며 ownership과 chain 불변식을 만족하는 살아 있는 writable `Queue`를 가리켜야 하고, 유효한 빈 queue는 허용한다.
## 사용할 개념
`malloc(sizeof *node)`, NULL 검사, FIFO, acyclic chain, head/tail/size 불변식.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o queue_enqueue
```
## 실행 방법
```sh
./queue_enqueue
```
## 예상 관찰 결과
note의 예제를 그대로 사용하고 allocation이 성공하면 다음과 같이 출력한다.
```text
size=2 first=10 last=20
empty=1 size=0
```

| scenario | 기대 결과 |
|---|---|
| empty -> enqueue(10) | head == tail, size 1, next NULL |
| single -> enqueue(20) | head는 10, tail은 20, size 2 |
| multiple -> enqueue(30) | 10,20,30 순서, tail->next NULL |
| allocation failure (injected) | 반환 0, head/tail/size/order 불변 |
| destroy -> empty -> reuse | 모두 NULL/0; 다시 삽입 가능 |
## 확인 포인트
`n`은 현재 원소 수이며 O(1)은 chain을 순회하지 않는 operation bound다. 실제 allocation 지연이나 CPU instruction 수의 보장은 아니다.
## 추가 실습
- ★ 한 node의 head와 tail 주소를 비교한다.
- ★★ head에서 세 node를 세어 size와 비교한다.
- ★★★ 실패 주입 후 동일한 순서로 재사용한다.
## 완료 기준
empty, single, multiple, failure, reuse에서 불변식과 해제 경로가 모두 맞는다.
