# 19-11. 학생 삭제
## 1. 학습 목표
- id로 record를 찾아 빈틈 없이 삭제한다.
- count와 유효 구간 invariant를 유지한다.
## 2. 선수 지식
19-10의 `[0,count)` invariant를 안다.
## 3. 핵심 개념
삭제 index 뒤의 elements를 한 칸 왼쪽으로 structure assignment하고 count를 감소시킨다.
## 4. 문법
```c
for (size_t j = index + 1; j < *count; ++j) a[j - 1] = a[j];
--*count;
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
#include <stddef.h>
typedef struct Student { int id; double score; } Student;
static int remove_id(Student a[], size_t *n, int id)
{
    size_t i = 0;
    while (i < *n && a[i].id != id) ++i;
    if (i == *n) return 0;
    for (size_t j = i + 1; j < *n; ++j) a[j - 1] = a[j];
    --*n; return 1;
}
int main(void)
{
    Student a[] = {{1,90},{2,80},{3,70}}; size_t n = 3;
    int ok = remove_id(a, &n, 2);
    printf("%d %zu %d\n", ok, n, a[1].id);
    return 0;
}
```
## 6. 코드 해석
id 2를 찾은 뒤 id 3 record를 왼쪽으로 이동하고 count를 2로 만든다.
## 7. 내부 동작
**[C17]** compatible structure assignment가 member values를 옮긴다. 삭제 뒤 `a[*n]`의 bytes가 남아 있어도 논리적 유효 구간 밖이다.
## 8. 자주 하는 실수
- 못 찾은 상태에서 count를 줄인다.
- loop에서 bounds 밖 `a[*n]`을 읽는다.
- 마지막 element 삭제를 별도 위험 코드로 만든다.
## 9. 필수 실습
첫·중간·마지막·없는 id 삭제를 검사한다.
[19-11 exercise](../../exercises/19-structures/19-11/README.md)
## 10. 추가 실습
- ★ 삭제 후 목록을 출력한다.
- ★★ 이동 횟수를 센다.
- ★★★ 순서를 보존하지 않는 삭제와 비교한다.
## 11. 확인 문제
1. 못 찾은 조건은?
2. 이동 loop의 첫/끝 index는?
3. 마지막 삭제 때 이동 횟수는?
4. count 밖 bytes는 유효 record인가?
## 12. 핵심 정리
- 찾은 뒤 이동하고 마지막에 count를 줄인다.
- 유효 구간은 항상 `[0,count)`다.
## 13. 다음 Step
[19-12. 학생 검색](19-12-student-search.md)
## 14. 참고 자료
- N1570 6.5.16.1, 6.5.6. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: assignment](https://en.cppreference.com/w/c/language/operator_assignment)
