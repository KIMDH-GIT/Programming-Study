# 28-4 실습: AddressSanitizer build와 실행
이론: [note](../../../notes/28-debugging/28-4-addresssanitizer-build-and-run.md)
## 실습 목적
ASan normal run과 expected diagnostic run을 분리한다.
## 작성할 파일
- `main.c`
- `/tmp/part28-validation/asan_oob.c`
## 해야 할 일
defined heap 사용을 정상 실행하고 작은 heap OOB는 임시 source에서만 관찰한다.
## 사용할 개념
ASan instrumentation, bounds, allocated lifetime.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -g -Og \
    -fsanitize=address main.c -o asan_defined
gcc -std=c17 -Wall -Wextra -Wpedantic -g -Og \
    -fsanitize=address /tmp/part28-validation/asan_oob.c \
    -o /tmp/part28-validation/asan_oob
```
## 실행 방법
```sh
./asan_defined
/tmp/part28-validation/asan_oob 2
```
## 예상 관찰 결과
정상 program은 `30`, invalid training run은 ASan memory-error diagnostic을 낸다. exact wording은 고정하지 않는다.
## 확인 포인트
expected ASan diagnostic을 normal run failure로 세지 않는다.
## 추가 실습
- ★ option 역할을 설명한다.
- ★★ valid/invalid index를 비교한다.
- ★★★ ASan 탐지와 C17 분류를 분리한다.
## 완료 기준
정상 실행과 expected ASan diagnostic을 각각 확인한다.
