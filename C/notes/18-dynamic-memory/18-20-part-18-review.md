# 18-20. Part 18 종합 복습

Part 18은 allocation size 검증부터 ownership·reallocation·cleanup까지 동적 메모리의 전체 lifetime을 다룬다.

## 1. 학습 목표
- malloc·calloc·realloc·free contract를 비교한다.
- overflow·failure·bounds·lifetime 오류를 분류한다.
- pointer+count+ownership 규율로 동적 배열을 관리한다.

## 2. 선수 지식
Step 18-1부터 18-19까지와 Part 14~17의 pointer 규칙을 사용한다.

## 3. 핵심 개념
안전한 흐름은 입력 검증 → size overflow 검사 → allocation → failure 검사 → initialization → bounded access → cleanup이다. `realloc`은 temporary pointer를 사용하고 성공 뒤 aliases를 다시 만든다.

## 4. 문법
```c
if (count == 0 || count > SIZE_MAX / sizeof *values) return 1;
values = malloc(count * sizeof *values);
if (values == NULL) return 1;
/* initialize and use within count */
free(values);
```

## 5. 최소 코드 예제
```c
#include <stdint.h>
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    size_t count = 3;
    if (count > SIZE_MAX / sizeof(int)) {
        return 1;
    }
    int *values = malloc(count * sizeof *values);
    if (values == NULL) {
        return 1;
    }
    for (size_t i = 0; i < count; ++i) {
        values[i] = (int)(i + 1);
    }

    int *temp = realloc(values, 4 * sizeof *values);
    if (temp == NULL) {
        free(values);
        return 1;
    }
    values = temp;
    values[3] = 4;
    printf("%d\n", values[3]);
    free(values);
    return 0;
}
```

## 6. 코드 해석
size와 failure를 검사해 allocation한다. initialized range만 사용하고 temporary pointer로 확장한 뒤 새 element를 초기화해 해제한다.

## 7. 내부 동작
- **[C17]** lifetime, bounds, UB와 library contracts를 규정한다.
- **[allocator]** block 관리 전략을 선택한다.
- **[OS]** virtual memory를 제공할 수 있다.
- **[CPU]** machine load/store를 실행한다.
어느 층도 pointer에 element count를 자동 첨부하지 않는다.

## 8. 자주 하는 실수
- malloc 결과가 zero initialized라고 생각한다.
- failure·overflow·UB·leak을 모두 runtime error라고 부른다.
- realloc 직접 대입과 old aliases를 사용한다.
- sizeof(pointer)를 block size로 생각한다.
- zero-size behavior를 한 가지로 단정한다.

## 9. 필수 실습
검증·allocation·초기화·realloc·bounded use·cleanup을 포함한 작은 동적 배열 프로그램을 작성한다.
[18-20 exercise](../../exercises/18-dynamic-memory/18-20/README.md)

## 10. 추가 실습
- ★ 오류 분류표를 만든다.
- ★★ ownership timeline을 그린다.
- ★★★ API별 성공·실패·cleanup contract를 작성한다.

## 11. 확인 문제
1. malloc storage 초기 내용은?
2. allocation-size overflow와 allocation failure 차이는?
3. `free(NULL)`과 double free 차이는?
4. calloc all-bits-zero를 모든 타입의 값 0으로 일반화할 수 있는가?
5. realloc failure-safe pattern은?
6. sizeof(pointer)가 block size를 주는가?
7. leak과 use-after-free의 behavior 분류는 어떻게 다른가?

## 12. 핵심 정리
- 크기·실패·bounds·lifetime을 각각 검증한다.
- ownership과 count를 pointer와 함께 관리한다.
- realloc 성공 뒤 반환 pointer만 사용한다.
- C17 contract와 allocator·OS 구현을 구분한다.

## 13. 다음 Step
커리큘럼의 다음 Step은 **19-1. 구조체 정의와 멤버**다. Part 19 파일은 만들지 않는다.

## 14. 참고 자료
- N1570 6.2.4; 6.5.6; 7.22.3. N1570은 **C11 공개 Committee Draft**이며 인용한 관련 규칙은 C17에서도 유지된다.
- [cppreference: dynamic memory management](https://en.cppreference.com/w/c/memory)
- [GCC: Instrumentation options](https://gcc.gnu.org/onlinedocs/gcc/Instrumentation-Options.html)
