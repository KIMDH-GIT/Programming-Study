# 30-14 실습: 메모리 누수 검증
이론: [note](../../../notes/30-data-structures/30-14-memory-leak-validation.md)
## 실습 목적
linked queue의 모든 정상/실패/재사용 경로에서 node 소유권을 정리한다.
## 작성할 파일
- `main.c` (학습자 작업 파일)
## 해야 할 일
enqueue/dequeue/destroy를 구현해 잔여 node를 전부 해제한다. dequeue의 non-NULL output은 호출과 반환까지 lifetime이 유지되는 writable `int` 객체를 가리키며 `Queue` 객체나 queue가 소유한 어떤 node와도 겹치지 않는다. 정리 함수의 인자는 NULL이 아니며 ownership과 chain 불변식을 만족하는 살아 있는 writable `Queue`를 가리켜야 하고, 유효한 빈 queue는 허용한다. 부분 삽입 실패 시 이전 node를 정리한다. leak, dangling pointer, use-after-free, double-free를 서로 다른 문제로 설명한다.
## 사용할 개념
`malloc`, `free`, lifetime, head/tail/size 불변식, sanitizer.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o queue_cleanup
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -g -O1 \
    -fsanitize=address,leak -fno-omit-frame-pointer main.c -o queue_san
```
## 실행 방법
```sh
./queue_cleanup
ASAN_OPTIONS=detect_leaks=1 ./queue_san
valgrind --leak-check=full --show-leak-kinds=all ./queue_cleanup
```
Valgrind는 설치되어 있을 때만 사용한다. sanitizer와 Valgrind 출력은 환경에 따라 다르다.
## 예상 관찰 결과
note의 성공 출력:
```text
out=10 remaining=1
empty=1
reuse_empty=1
```

| scenario | 기대 결과 |
|---|---|
| empty destroy | head/tail NULL, size 0 |
| single destroy | node 한 번 해제, empty |
| multiple + partial dequeue | 남은 chain 전부 해제 |
| second enqueue failure | 첫 node 해제하고 정상 종료 |
| destroy -> reuse -> destroy | 두 실행 구간의 모든 node 해제 |
## 확인 포인트
`n`은 현재 원소 수이며 destroy의 O(n)은 방문 횟수다. ASan/LeakSanitizer/Valgrind clean은 실행한 경로의 관찰이며 C17 보장이나 모든 경로의 증명이 아니다.
## 추가 실습
- ★ 빈 상태 destroy를 반복한다.
- ★★ 단일/여러 node의 해제 횟수를 기록한다.
- ★★★ 사용 가능한 leak 도구의 실행 결과를 비교한다.
## 완료 기준
strict 실행 결과가 일치하고 지원 환경의 leak 도구에서 이 정상 경로의 오류 보고가 없다.
