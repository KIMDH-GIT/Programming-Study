# 28-5 실습: ASan report 읽기
이론: [note](../../../notes/28-debugging/28-5-reading-asan-reports-and-limitations.md)
## 실습 목적
ASan report에서 source-level 원인을 찾고 수정한다.
## 작성할 파일
- `/tmp/part28-validation/asan_oob.c`
- `main.c`
## 해야 할 일
임시 OOB report의 category와 첫 user frame을 기록하고 valid bound로 수정한다.
## 사용할 개념
ASan report, access type, source location, bounds.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -g -Og \
    -fsanitize=address /tmp/part28-validation/asan_oob.c \
    -o /tmp/part28-validation/asan_oob
gcc -std=c17 -Wall -Wextra -Wpedantic -g -Og \
    -fsanitize=address main.c -o asan_fixed
```
## 실행 방법
```sh
/tmp/part28-validation/asan_oob 2
./asan_fixed
```
## 예상 관찰 결과
첫 실행은 expected ASan diagnostic, 수정본은 `20`과 정상 종료를 보인다.
## 확인 포인트
exact address·wording 대신 category와 source location을 확인한다.
## 추가 실습
- ★ read/write 종류를 찾는다.
- ★★ allocation context를 찾는다.
- ★★★ clean run의 한계를 설명한다.
## 완료 기준
report 근거로 bound를 고치고 ASan-instrumented 수정본을 재검증한다.
