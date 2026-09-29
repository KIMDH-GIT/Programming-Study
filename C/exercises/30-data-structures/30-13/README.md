# 30-13 실습: allocation 실패와 구조 보존
이론: [note](../../../notes/30-data-structures/30-13-allocation-failure-and-state-preservation.md)
## 실습 목적
할당 실패가 기존 queue를 변경하지 않는다는 계약을 결정적으로 검사한다.
## 작성할 파일
- `main.c` (학습자 작업 파일)
## 해야 할 일
테스트용으로 한 번 NULL을 반환하는 작은 allocator 함수를 만들고, empty와 multiple 상태에서 실패를 주입한다. enqueue는 NULL이 아닌 유효한 `Queue`와 `size < SIZE_MAX`를 precondition으로 받으며 성공 후에만 링크/size를 수정한다. 실패 전후 head/tail 주소, size, 값 순서와 tail->next를 비교하고, 실패 후 재삽입/해제를 수행한다. 정리 함수의 인자는 NULL이 아니며 ownership과 chain 불변식을 만족하는 살아 있는 writable `Queue`를 가리켜야 하고, 유효한 빈 queue는 허용한다.
## 사용할 개념
NULL, allocation, pointer identity, FIFO, 불변식.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o queue_failure
```
## 실행 방법
```sh
./queue_failure
```
## 예상 관찰 결과
note의 주입 예제를 실행하면 다음과 같다.
```text
empty_failure=1 empty_unchanged=1
failure=1 preserved=1
reuse=10,20,30 size=3
```

| scenario | 기대 결과 |
|---|---|
| empty + injected failure | 0 반환, head/tail NULL, size 0 |
| single + injected failure | head == tail, 기존 값과 size 1 보존 |
| multiple + injected failure | 주소/순서/size 2, tail->next NULL 유지 |
| failure 이후 success | 새 tail에만 링크, FIFO 유지 |
| destroy -> reuse | 빈 세 필드로 복구한 후 다시 삽입 가능 |
## 확인 포인트
실제 malloc 실패가 이 환경에서 발생하기를 기대하지 않는다. 주입은 실패 경로 검증용이고 malloc/OS 동작의 증명은 아니다.
## 추가 실습
- ★ 빈 상태에서 실패를 주입한다.
- ★★ 한 node와 여러 node의 보존을 각각 확인한다.
- ★★★ 실패 후 성공과 destroy/reuse까지 검사한다.
## 완료 기준
주입 실패에서 mutation이 없고 정상 성공/정리/재사용이 모두 동작한다.
