# 19-13. 학생 목록
## 1. 학습 목표
- 유효 records만 일관된 형식으로 출력한다.
- 빈 목록을 구분한다.
## 2. 선수 지식
19-4 순회와 const parameter를 안다.
## 3. 핵심 개념
목록 함수는 `[0,count)`만 읽으며 수정하지 않는다. 출력 형식은 한 record의 fields가 분명해야 한다.
## 4. 문법
```c
void print_students(const Student students[], size_t count);
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
typedef struct Student { int id; double score; } Student;
static void print_students(const Student a[], size_t n)
{
    if (n == 0) { puts("empty"); return; }
    for (size_t i = 0; i < n; ++i)
        printf("%d %.1f\n", a[i].id, a[i].score);
}
int main(void)
{
    Student a[] = {{1,90.0},{2,80.5}};
    print_students(a, sizeof a / sizeof a[0]);
    return 0;
}
```
## 6. 코드 해석
count가 0이면 element를 읽지 않고, 아니면 각 structure element의 members를 출력한다.
## 7. 내부 동작
**[C17]** `printf` format과 argument type이 일치해야 한다. array parameter는 pointer이므로 함수 안에서 전체 array 크기를 복원할 수 없다.
## 8. 자주 하는 실수
- capacity 전체를 출력한다.
- 빈 목록에서 element 0을 읽는다.
- `%d`와 `double`처럼 format을 틀린다.
## 9. 필수 실습
빈 목록과 여러 학생 목록을 같은 함수로 출력한다.
[19-13 exercise](../../exercises/19-structures/19-13/README.md)
## 10. 추가 실습
- ★ header를 출력한다.
- ★★ 평균을 마지막에 출력한다.
- ★★★ 출력 함수와 검색 로직을 분리한다.
## 11. 확인 문제
1. 유효 구간은?
2. capacity를 출력하면 안 되는 이유는?
3. const의 의미는?
4. format mismatch의 위험은?
## 12. 핵심 정리
- 목록은 count까지만 읽는다.
- 출력 함수는 data를 수정하지 않는다.
## 13. 다음 Step
[19-14. 학생 정렬](19-14-student-sort.md)
## 14. 참고 자료
- N1570 6.7.6.3, 7.21.6.1. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: printf](https://en.cppreference.com/w/c/io/fprintf)
