# 18-15. double free와 `NULL` dereference

같은 live allocation을 두 번 해제하는 것과 null pointer를 dereference하는 것은 서로 다른 undefined behavior 원인이다.

## 1. 학습 목표
- double free와 `free(NULL)`을 구분한다.
- null dereference를 allocation failure와 연결한다.
- 특정 allocator error message를 표준 결과로 단정하지 않는다.

## 2. 선수 지식
Step 18-7의 NULL 검사와 Step 18-8의 free contract를 안다.

## 3. 핵심 개념
`free(p); free(p);`의 두 번째 call은 이미 deallocated된 pointer를 전달해 UB다. 반면 `free(NULL)`은 no-op이다. `*NULL` access도 UB이며 `free(NULL)`의 안전성과 혼동하면 안 된다.

## 4. 문법
```c
free(p);
p = NULL;
```
한 owner pointer의 accidental second free 위험을 줄이지만 aliases는 별도 관리해야 한다.

## 5. 최소 코드 예제
```c
#include <stdlib.h>

int main(void)
{
    int *p = malloc(sizeof *p);

    if (p == NULL) {
        return 1;
    }
    *p = 9;
    free(p);
    p = NULL;
    free(p);
    return 0;
}
```

## 6. 코드 해석
첫 call은 allocation을 해제한다. null 대입 뒤 두 번째 call은 `free(NULL)`이므로 유효하다. null dereference는 하지 않는다.

## 7. 내부 동작
allocator가 "double free detected"를 출력할 수도 있지만 C17은 그런 diagnostic을 보장하지 않는다. null dereference도 특정 signal을 보장하지 않는다.

## 8. 자주 하는 실수
- `free(NULL)`과 double free가 같다고 생각한다.
- `p = NULL`을 해제 자체로 착각한다.
- null pointer를 읽기만 dereference하면 괜찮다고 생각한다.

## 9. 필수 실습
안전한 single-free pattern을 작성하고 double-free/null-dereference fragments를 실행 없이 분류한다.
[18-15 exercise](../../exercises/18-dynamic-memory/18-15/README.md)

## 10. 추가 실습
- ★ valid `free` arguments를 표로 만든다.
- ★★ owner를 하나로 제한하는 contract를 작성한다.
- ★★★ alias가 있는 double-free 시나리오를 분석한다.

## 11. 확인 문제
1. `free(NULL)`은 무엇을 하는가?
2. 같은 allocation을 두 번 free하면 어떤 분류인가?
3. null dereference와 free(NULL)은 왜 다른가?
4. 특정 error message가 보장되는가?

## 12. 핵심 정리
- live allocation은 정확히 한 번 해제한다.
- `free(NULL)`은 허용되지만 dereference는 아니다.
- allocator diagnostic과 C17 semantics를 구분한다.

## 13. 다음 Step
[18-16. 동적 배열 out-of-bounds](18-16-dynamic-array-out-of-bounds.md)

## 14. 참고 자료
- N1570 6.5.3.2 Address and indirection operators; 7.22.3.3 `free`. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: free](https://en.cppreference.com/w/c/memory/free)
- [SEI CERT MEM30-C](https://wiki.sei.cmu.edu/confluence/display/c/MEM30-C.+Do+not+access+freed+memory)
