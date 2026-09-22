# 19-15. 학생 관리 프로그램 통합
## 1. 학습 목표
- 검증, 등록, 삭제, 검색, 목록, 정렬을 통합한다.
- array invariant와 함수 책임을 유지한다.
## 2. 선수 지식
19-8부터 19-14의 모든 기능을 안다.
## 3. 핵심 개념
`students`, `count`, `capacity`를 함께 관리한다. 입력 경계에서 parse하고 내부 함수에는 검증된 값을 전달한다.
## 4. 문법
```c
switch (command) {
case 1: /* add */ break;
case 2: /* delete */ break;
case 3: /* search */ break;
case 4: /* list */ break;
case 5: /* sort */ break;
case 0: /* quit */ break;
default: /* invalid */ break;
}
```
Part 20의 enum·union은 사용하지 않는다.
## 5. 최소 코드 예제
```c
#include <errno.h>
#include <stdio.h>
#include <stdlib.h>
typedef struct Student { int id; double score; } Student;
static size_t find_id(const Student a[], size_t n, int id)
{
    for (size_t i = 0; i < n; ++i) if (a[i].id == id) return i;
    return n;
}
static int add(Student a[], size_t *n, size_t cap, Student s)
{
    if (*n == cap || find_id(a, *n, s.id) < *n) return 0;
    a[(*n)++] = s; return 1;
}
static int remove_id(Student a[], size_t *n, int id)
{
    size_t i = find_id(a, *n, id);
    if (i == *n) return 0;
    for (++i; i < *n; ++i) a[i - 1] = a[i];
    --*n; return 1;
}
static void sort_by_id(Student a[], size_t n)
{
    for (size_t i = 0; i < n; ++i)
        for (size_t j = i + 1; j < n; ++j)
            if (a[j].id < a[i].id) {
                Student t = a[i]; a[i] = a[j]; a[j] = t;
            }
}
static void list(const Student a[], size_t n)
{
    for (size_t i = 0; i < n; ++i)
        printf("%d %.1f\n", a[i].id, a[i].score);
}
static int read_command(void)
{
    char text[16], *end;
    if (fgets(text, sizeof text, stdin) == NULL) return 0;
    errno = 0;
    long command = strtol(text, &end, 10);
    if (end == text || errno == ERANGE
        || (*end != '\n' && *end != '\0')
        || (*end == '\n' && end[1] != '\0')
        || command < 0 || command > 5) return -1;
    return (int)command;
}
int main(void)
{
    Student students[3] = {{1,90.0},{2,80.0}};
    size_t count = 2;
    for (;;) {
        puts("1:add 2:delete 3:search 4:list 5:sort 0:quit");
        int command = read_command();
        if (command == 0) return 0;
        switch (command) {
        case 1: puts(add(students, &count, 3, (Student){3, 70.0}) ? "added" : "add failed"); break;
        case 2: puts(remove_id(students, &count, 1) ? "deleted" : "not found"); break;
        case 3: {
            size_t i = find_id(students, count, 2);
            if (i < count) printf("found %.1f\n", students[i].score);
            else puts("not found");
            break;
        }
        case 4: list(students, count); break;
        case 5: sort_by_id(students, count); puts("sorted"); break;
        default: puts("invalid"); break;
        }
    }
}
```
## 6. 코드 해석
하나의 `students`·`count` 상태를 등록·삭제·검색·목록·정렬 함수가 공유한다. 입력 문자열은 `read_command` 경계에서 검증되고 `0` 또는 EOF가 반복을 끝낸다. 고정 record 값은 예제를 작게 유지하기 위한 것이며 완성 실습에서는 19-8·19-9 parser로 입력한다.
## 7. 내부 동작
**[C17]** 입력 parse, bounds, lifetime, format contracts를 각 경계에서 지킨다. raw structure bytes를 파일/network에 쓰는 portable serialization은 아니다.
## 8. 자주 하는 실수
- 모든 기능을 `main`에 중복한다.
- 검증 실패 값을 record에 먼저 저장한다.
- count와 capacity를 혼동한다.
- 후속 Part의 enum·union 또는 파일 저장을 미리 섞는다.
## 9. 필수 실습
반복 메뉴에서 add/delete/search/list/sort/quit를 연결한다.
[19-15 exercise](../../exercises/19-structures/19-15/README.md)
## 10. 추가 실습
- ★ 오류 메시지를 일관되게 한다.
- ★★ 함수 contract를 표로 쓴다.
- ★★★ 정상 시나리오 transcript를 만든다.
## 11. 확인 문제
1. 입력 검증 책임은 어디에 있는가?
2. 핵심 array invariant는?
3. 기능별 함수 분리의 이점은?
4. Part 20 문법을 쓰지 않는 이유는?
5. raw bytes 저장이 portable하지 않은 이유는?
## 12. 핵심 정리
- 검증된 값만 data model에 넣는다.
- 각 함수가 하나의 invariant-preserving operation을 담당한다.
## 13. 다음 Step
[19-16. Part 19 종합 복습](19-16-part-19-review.md)
## 14. 참고 자료
- N1570 5.1.2.3, 6.5, 7.22. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [SEI CERT C Coding Standard](https://wiki.sei.cmu.edu/confluence/display/c/SEI+CERT+C+Coding+Standard)
