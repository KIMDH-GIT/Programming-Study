# 19-12. 학생 검색
## 1. 학습 목표
- id로 record를 검색한다.
- found/not-found를 모호하지 않게 표현한다.
## 2. 선수 지식
구조체 배열과 const pointer parameter를 안다.
## 3. 핵심 개념
검색 함수는 배열을 수정하지 않으므로 `const Student[]`를 받고 index를 반환한다. `SIZE_MAX`를 not-found sentinel로 쓸 수 있다.
## 4. 문법
```c
size_t find_id(const Student a[], size_t count, int id);
```
## 5. 최소 코드 예제
```c
#include <stdint.h>
#include <stdio.h>
typedef struct Student { int id; double score; } Student;
static size_t find_id(const Student a[], size_t n, int id)
{
    for (size_t i = 0; i < n; ++i) if (a[i].id == id) return i;
    return SIZE_MAX;
}
int main(void)
{
    Student a[] = {{1,90},{2,80}};
    size_t i = find_id(a, 2, 2);
    if (i != SIZE_MAX) printf("%.1f\n", a[i].score);
    return 0;
}
```
## 6. 코드 해석
sentinel을 확인한 뒤에만 index로 사용한다.
## 7. 내부 동작
**[C17]** array parameter는 pointer로 adjusted되므로 count를 별도로 전달한다. const는 함수가 그 경로로 records를 수정하지 않는 interface를 표현한다.
## 8. 자주 하는 실수
- not-found index를 바로 사용한다.
- count 없이 끝을 추측한다.
- 검색 함수에서 배열을 수정한다.
## 9. 필수 실습
있는 id와 없는 id를 검색해 결과를 구분한다.
[19-12 exercise](../../exercises/19-structures/19-12/README.md)
## 10. 추가 실습
- ★ score 조건 검색을 한다.
- ★★ pointer 반환 방식과 비교한다.
- ★★★ 중복이 없다는 invariant를 활용한다.
## 11. 확인 문제
1. count가 필요한 이유는?
2. sentinel 확인 시점은?
3. const가 표현하는 contract는?
4. array parameter에서 `sizeof`로 count를 얻을 수 있는가?
## 12. 핵심 정리
- search result를 확인한 뒤 사용한다.
- 읽기 전용 interface와 count를 명시한다.
## 13. 다음 Step
[19-13. 학생 목록](19-13-student-list.md)
## 14. 참고 자료
- N1570 6.7.6.3, 7.20.2. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: array declaration](https://en.cppreference.com/w/c/language/array)
