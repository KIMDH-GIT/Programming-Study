# 18-19. 동적 배열 정렬

allocated array도 pointer와 count로 범위를 전달하면 기존 배열 알고리즘을 적용할 수 있다.

## 1. 학습 목표
- dynamic array에 in-place 정렬을 적용한다.
- 정렬 함수가 storage를 빌려 쓸 뿐 free하지 않음을 명시한다.
- bounds와 ownership을 유지한다.

## 2. 선수 지식
Part 11의 배열, Part 16의 array parameter, Step 18-18의 allocation을 안다.

## 3. 핵심 개념
정렬 함수의 parameter는 adjusted pointer다. caller가 allocation과 deallocation 책임을 유지하고 callee는 `count` 범위의 elements만 수정한다.

## 4. 문법
```c
static void sort(int values[], size_t count);
```

## 5. 최소 코드 예제
```c
#include <stdio.h>
#include <stdlib.h>

static void sort(int values[], size_t count)
{
    for (size_t end = count; end > 1; --end) {
        for (size_t i = 1; i < end; ++i) {
            if (values[i - 1] > values[i]) {
                int temp = values[i - 1];
                values[i - 1] = values[i];
                values[i] = temp;
            }
        }
    }
}

int main(void)
{
    size_t count = 4;
    int *values = malloc(count * sizeof *values);
    if (values == NULL) {
        return 1;
    }
    values[0] = 4; values[1] = 1; values[2] = 3; values[3] = 2;
    sort(values, count);
    for (size_t i = 0; i < count; ++i) {
        printf("%d%c", values[i], i + 1 == count ? '\n' : ' ');
    }
    free(values);
    return 0;
}
```

## 6. 코드 해석
callee는 count 범위에서 elements만 교환한다. caller는 base pointer를 유지하고 출력 뒤 해제한다.

## 7. 내부 동작
allocated storage라고 특별한 sorting instruction이 생기지 않는다. CPU는 compiler가 생성한 load/store/compare를 수행하며 allocator는 정렬에 관여하지 않는다.

## 8. 자주 하는 실수
- 정렬 함수가 borrowed pointer를 free한다.
- loop bound에서 count 밖을 읽는다.
- sort 뒤 base pointer 대신 interior pointer를 free한다.

## 9. 필수 실습
작은 fixed count의 values를 allocation해 bubble sort하고 출력·해제한다.
[18-19 exercise](../../exercises/18-dynamic-memory/18-19/README.md)

## 10. 추가 실습
- ★ 내림차순으로 바꾼다.
- ★★ 이미 정렬된 입력을 확인한다.
- ★★★ 정렬 함수 ownership contract를 문서화한다.

## 11. 확인 문제
1. 정렬 함수가 allocation을 free해야 하는가?
2. array parameter는 실제로 어떤 parameter로 조정되는가?
3. count가 필요한 이유는?
4. 정렬 뒤 무엇을 free해야 하는가?

## 12. 핵심 정리
- dynamic array도 pointer+count로 알고리즘에 전달한다.
- borrower는 ownership을 넘겨받지 않는다.
- caller가 base pointer를 정확히 한 번 해제한다.

## 13. 다음 Step
[18-20. Part 18 종합 복습](18-20-part-18-review.md)

## 14. 참고 자료
- N1570 6.7.6.3 Function declarators; 7.22.3.4 `malloc`. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: array declarations](https://en.cppreference.com/w/c/language/array)
- [cppreference: malloc](https://en.cppreference.com/w/c/memory/malloc)
