# 19-16. Part 19 종합 복습
## 1. 학습 목표
- 구조체 문법과 학생 관리 invariant를 종합한다.
- C17 semantics와 구현 detail을 구분한다.
## 2. 선수 지식
19-1부터 19-15까지를 학습했다.
## 3. 핵심 개념
구조체는 named members를 가진 aggregate type이다. object/member, tag/typedef, value/pointer parameter를 구분하고 `[0,count)` invariant 위에서 학생 records를 관리한다.
## 4. 문법
```c
typedef struct Student { int id; double score; } Student;
Student students[10];
students[0].id = 1;
Student *p = &students[0];
p->score = 90.0;
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
typedef struct Student { int id; double score; } Student;
static void raise_score(Student *s, double amount)
{
    s->score += amount;
    if (s->score > 100.0) s->score = 100.0;
}
int main(void)
{
    Student students[] = {{2,80.0},{1,90.0}};
    Student copy = students[0];
    raise_score(&copy, 5.0);
    printf("%d %.1f\n", copy.id, copy.score);
    return 0;
}
```
## 6. 코드 해석
structure assignment로 독립 record value를 복사하고 pointer parameter로 복사본의 member를 수정한다.
## 7. 내부 동작
- **[C17]** member order, compatible assignment, pass-by-value와 access semantics를 정한다.
- **[compiler/ABI]** padding, alignment, parameter/return 전달 구현을 정한다.
- **[CPU]** 실제 instructions를 실행한다.
`sizeof(struct)`은 member 합과 다를 수 있고 exact layout은 구현 관찰값이다. pointer member 복사는 pointer value만 복사하는 shallow copy이며 allocation을 deep-copy하지 않는다. 두 복사본을 각각 owner라 오해해 같은 pointer를 free하면 UB인 double free 위험이 있다.
## 8. 자주 하는 실수
- `Point`를 typedef 없이 type name으로 쓴다.
- structure `==`, 자동 deep copy, call by reference를 가정한다.
- padding 크기와 ABI 전달 방식을 C17 보장으로 말한다.
- pointer member ownership을 정하지 않는다.
## 9. 필수 실습
구조체 선언·배열·함수·입력 검증·CRUD·정렬을 포함한 Part 19 프로그램을 점검한다.
[19-16 exercise](../../exercises/19-structures/19-16/README.md)
## 10. 추가 실습
- ★ 핵심 용어 표를 만든다.
- ★★ value/pointer 흐름을 그린다.
- ★★★ pointer member shallow-copy 위험을 실행 없이 분석한다.
## 11. 확인 문제
1. tag와 typedef name 차이는?
2. `.`와 `->`의 대상은?
3. structure assignment와 equality의 차이는?
4. pass-by-value인데 pointer 함수가 원본을 바꾸는 이유는?
5. padding/alignment의 exact 값은 누가 정하는가?
6. pointer member의 shallow copy란?
7. 학생 배열 invariant는?
## 12. 핵심 정리
- structure value와 pointer를 구분한다.
- bounds, lifetime, input validation, ownership을 명시한다.
- C17 보장과 compiler·ABI·CPU 구현을 분리한다.
## 13. 다음 Step
커리큘럼의 다음 Step은 **20-1. `enum`과 열거 상수**다. Part 20 파일은 만들지 않는다.
## 14. 참고 자료
- N1570 6.2.3, 6.5.2.3, 6.5.16.1, 6.7.2.1, 6.7.8, 6.7.9, 7.22.1. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: structures](https://en.cppreference.com/w/c/language/struct)
- [GCC C implementation-defined behavior](https://gcc.gnu.org/onlinedocs/gcc/C-Implementation.html)
