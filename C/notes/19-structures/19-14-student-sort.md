# 19-14. 학생 정렬
## 1. 학습 목표
- structure 전체를 교환해 id 순으로 정렬한다.
- 유효 구간 밖을 건드리지 않는다.
## 2. 선수 지식
배열 정렬과 structure assignment를 안다.
## 3. 핵심 개념
key member만 바꾸면 record fields의 관계가 깨진다. 임시 structure object로 record 전체를 교환한다.
## 4. 문법
```c
Student temp = a[i];
a[i] = a[j];
a[j] = temp;
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
typedef struct Student { int id; double score; } Student;
static void sort_by_id(Student a[], size_t n)
{
    for (size_t i = 0; i < n; ++i)
        for (size_t j = i + 1; j < n; ++j)
            if (a[j].id < a[i].id) {
                Student t = a[i]; a[i] = a[j]; a[j] = t;
            }
}
int main(void)
{
    Student a[] = {{3,70},{1,90},{2,80}};
    sort_by_id(a, 3);
    for (size_t i = 0; i < 3; ++i) printf("%d %.0f\n", a[i].id, a[i].score);
    return 0;
}
```
## 6. 코드 해석
비교는 id member로 하지만 교환은 Student value 전체로 한다.
## 7. 내부 동작
**[C17]** structure assignment는 array member가 있어도 structure value 전체에 적용된다. pointer member가 있다면 pointer value만 복사되며 pointed allocation의 automatic deep copy는 아니다.
## 8. 자주 하는 실수
- id만 교환해 score와 분리한다.
- inner loop를 `j <= n`으로 쓴다.
- structure copy가 pointer 대상까지 deep-copy한다고 생각한다.
## 9. 필수 실습
세 record를 id 오름차순으로 정렬하고 record 결합을 확인한다.
[19-14 exercise](../../exercises/19-structures/19-14/README.md)
## 10. 추가 실습
- ★ score 내림차순을 만든다.
- ★★ 같은 score의 tie-break를 정한다.
- ★★★ 정렬 전후 id-score 쌍을 검증한다.
## 11. 확인 문제
1. 왜 record 전체를 교환하는가?
2. loop bounds는?
3. array member도 assignment 대상인가?
4. pointer member assignment가 deep copy인가?
## 12. 핵심 정리
- key는 비교 기준이고 record 전체가 이동 단위다.
- structure assignment는 typed value assignment다.
## 13. 다음 Step
[19-15. 학생 관리 프로그램 통합](19-15-student-management-integration.md)
## 14. 참고 자료
- N1570 6.5.16.1. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: assignment](https://en.cppreference.com/w/c/language/operator_assignment)
