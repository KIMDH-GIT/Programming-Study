# 23-14. Part 23 종합 복습
## 1. 학습 목표
- stream lifecycle, text parsing, binary block I/O, persistence를 종합한다.
- C17 stdio와 OS·filesystem·device 층을 구분한다.
- malformed input과 partial I/O를 안전하게 처리한다.
## 2. 선수 지식
23-1부터 23-13까지를 학습했다.
## 3. 핵심 개념
안전한 file I/O 흐름은 다음과 같다.
```text
format contract 결정
→ 올바른 mode로 fopen
→ open failure 검사
→ 각 read/write 결과 검사
→ EOF·error·parse failure 구분
→ fclose 결과 검사
```
`FILE *`는 C stream interface이고 POSIX descriptor가 아니다. raw struct bytes는 portable serialization이 아니다.
## 4. 문법
```c
FILE *fp = fopen("part23_review.txt", "r");
if (fp == NULL) {
    /* open failure */
}
```
character input은 `int`, string input은 capacity, formatted input은 assignment count, block I/O는 element count를 검사한다.
## 5. 최소 코드 예제
```c
#include <errno.h>
#include <limits.h>
#include <math.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct Student {
    int id;
    double score;
    char name[20];
};

int parse_student(const char *line, struct Student *student)
{
    char *end;
    char *score_start;
    char extra;
    long id;
    double score;

    errno = 0;
    id = strtol(line, &end, 10);
    if (end == line || errno == ERANGE ||
        id <= 0 || id > INT_MAX) {
        return 0;
    }
    score_start = end;
    errno = 0;
    score = strtod(score_start, &end);
    if (end == score_start || errno == ERANGE ||
        !isfinite(score) || score < 0.0 || score > 100.0 ||
        sscanf(end, " %19s %c", student->name, &extra) != 1) {
        return 0;
    }
    student->id = (int)id;
    student->score = score;
    return 1;
}

int main(void)
{
    const struct Student saved = {1001, 88.5, "Park"};
    struct Student loaded;
    char line[128];
    FILE *fp = fopen("part23_review.txt", "w");

    if (fp == NULL) {
        return 1;
    }
    if (fprintf(fp, "%d %.2f %s\n",
                saved.id, saved.score, saved.name) < 0) {
        fclose(fp);
        return 1;
    }
    if (fclose(fp) == EOF) {
        return 1;
    }

    fp = fopen("part23_review.txt", "r");
    if (fp == NULL ||
        fgets(line, sizeof line, fp) == NULL ||
        strchr(line, '\n') == NULL ||
        !parse_student(line, &loaded)) {
        if (fp != NULL) {
            fclose(fp);
        }
        return 1;
    }
    if (fclose(fp) == EOF) {
        return 1;
    }
    printf("%d %.2f %s\n",
           loaded.id, loaded.score, loaded.name);
    return 0;
}
```
## 6. 코드 해석
student fields를 explicit text format으로 저장하고 bounded line parsing으로 복원한다. line completion, exact fields, representability, finite score, ranges를 검사하고 모든 streams를 닫는다.
## 7. 내부 동작
**[C17 stdio stream]** stream mode, position, indicators, text/binary semantics가 C-level behavior를 정한다.

**[C standard library]** formatted·character·line·block I/O APIs와 return contracts를 제공한다.

**[OS file / file descriptor]** POSIX `open`·`read`·`write`·`close`와 `int fd`는 별도 interface다.

**[filesystem]** names, paths, permissions, persistence는 host environment 영향을 받는다.

**[device / storage]** stdio buffering과 physical durability는 같은 보장이 아니다.

**[MIPS — 수업 기준]** 교육용 simulator syscall interface는 ISO C stdio가 아니다.

**[RISC-V — 병행 학습]** environment-specific `ecall`도 C17 `fopen` semantics 자체가 아니다.
## 8. 자주 하는 실수
- `FILE *`와 file descriptor를 같다고 말한다.
- `"w"` truncation과 `"a"` append semantics를 바꿔 이해한다.
- `char`로 `fgetc` result를 받고 `while (!feof(fp))`를 쓴다.
- `fgets`가 newline을 제거하고 항상 line 전체를 준다고 생각한다.
- `fread` return을 항상 byte count라고 생각한다.
- raw struct write를 portable serialization이라고 부른다.
- `fflush(stdin)` 또는 `fflush` disk guarantee를 주장한다.
- `ftell`을 모든 stream에서 portable byte file size라고 일반화한다.
## 9. 필수 실습
student record 하나를 explicit text format으로 저장·복원하고 모든 results를 검사한다.
[23-14 exercise](../../exercises/23-file-io/23-14/README.md)
## 10. 추가 실습
- ★ 여러 records를 저장·복원한다.
- ★★ malformed record가 있을 때 실패 위치를 출력한다.
- ★★★ versioned format과 migration 정책을 설계한다.
## 11. 확인 문제
1. `FILE *`와 OS descriptor의 차이는?
2. open mode가 정하는 세 가지 핵심 behavior는?
3. EOF와 read error를 어떻게 구분하는가?
4. line input과 block input의 return contract 차이는?
5. raw struct bytes가 portable하지 않은 이유는?
6. update stream 방향 전환에는 어떤 규칙이 필요한가?
7. stdio flush와 device durability가 다른 이유는?
## 12. 핵심 정리
- every open·read·write·close result를 검사한다.
- stream state와 application format validation을 함께 관리한다.
- C stdio, OS, filesystem, storage 층을 분리한다.
## 13. 다음 Step
Part 24의 첫 Step은 **24-1. 기능별 source file 분리**이다. 이번 Part에서는 Part 24 파일을 만들지 않는다.
## 14. 참고 자료
- N1570 7.21 전체, 특히 7.21.2~7.21.10. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: C file input/output](https://en.cppreference.com/w/c/io)
- GCC official documentation and verified systems programming textbooks
