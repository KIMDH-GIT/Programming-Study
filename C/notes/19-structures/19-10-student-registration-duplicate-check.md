# 19-10. 학생 등록과 중복 검사
## 1. 학습 목표
- capacity와 중복 id를 검사한 뒤 record를 추가한다.
- 실패 시 기존 배열을 보존한다.
## 2. 선수 지식
구조체 배열과 검증된 입력을 안다.
## 3. 핵심 개념
등록 전 `count < capacity`와 기존 `id` 부재를 확인한다. 검증이 끝난 뒤 `students[count]`에 저장하고 마지막에 count를 증가시킨다.
## 4. 문법
```c
int find_id(const Student a[], size_t n, int id);
int add(Student a[], size_t *n, size_t cap, Student value);
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
#include <stddef.h>
typedef struct Student { int id; double score; } Student;
static int add(Student a[], size_t *n, size_t cap, Student s)
{
    if (*n == cap) return 0;
    for (size_t i = 0; i < *n; ++i) if (a[i].id == s.id) return 0;
    a[*n] = s;
    ++*n;
    return 1;
}
int main(void)
{
    Student a[2] = {{1, 90.0}}; size_t n = 1;
    int added = add(a, &n, 2, (Student){2, 80.0});
    printf("%d %zu\n", added, n);
    return 0;
}
```
## 6. 코드 해석
중복·용량 검사 뒤 structure assignment로 새 record를 저장한다.
## 7. 내부 동작
**[C17]** array bounds와 pointer 유효성이 전제다. 상태를 바꾸는 함수 호출과 상태 출력은 별도 statement로 나누어 평가 순서에 의존하지 않는다.
## 8. 자주 하는 실수
- 저장 후 중복을 발견한다.
- capacity를 넘긴다.
- 실패했는데 count를 증가시킨다.
## 9. 필수 실습
중복과 full 상태에서 배열이 변하지 않는 등록 함수를 작성한다.
[19-10 exercise](../../exercises/19-structures/19-10/README.md)
## 10. 추가 실습
- ★ 결과 code를 출력한다.
- ★★ 이름 배열 member를 추가한다.
- ★★★ invariant를 주석으로 쓴다.
## 11. 확인 문제
1. count invariant는?
2. 중복 검사는 언제 하는가?
3. 실패 시 무엇이 보존되어야 하는가?
4. 저장에 structure assignment를 쓸 수 있는가?
## 12. 핵심 정리
- 검사 완료 뒤 한 번만 commit한다.
- `[0,count)`만 유효 record다.
## 13. 다음 Step
[19-11. 학생 삭제](19-11-student-deletion.md)
## 14. 참고 자료
- N1570 6.5.16.1, 6.5.6. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [SEI CERT ARR30-C](https://wiki.sei.cmu.edu/confluence/display/c/ARR30-C.+Do+not+form+or+use+out-of-bounds+pointers+or+array+subscripts)
