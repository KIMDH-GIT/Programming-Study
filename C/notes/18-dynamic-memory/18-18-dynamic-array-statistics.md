# 18-18. 동적 배열 입력·평균·최댓값·최솟값

사용자 count와 element 입력을 검증하고 allocated array의 통계를 계산한다.

## 1. 학습 목표
- 입력 성공·0·overflow·allocation failure를 순서대로 검사한다.
- dynamic array의 평균·최댓값·최솟값을 계산한다.
- 모든 종료 경로에서 ownership을 정리한다.

## 2. 선수 지식
Step 18-4의 overflow, Step 18-6의 array pattern, Part 6의 input 검사를 안다.

## 3. 핵심 개념
신뢰 경계인 사용자 입력은 allocation 전에 검증한다. `scanf`의 정수 변환은 입력 값이 destination type으로 표현되지 않으면 C17에서 undefined behavior가 될 수 있으므로, bounded line을 읽고 `strtoumax`·`strtoimax`의 range 결과를 확인한다. count의 leading `-`도 명시적으로 거부한다.

## 4. 문법
```c
static int read_count(size_t *result);
static int read_int(int *result);
```

## 5. 최소 코드 예제
```c
#include <ctype.h>
#include <errno.h>
#include <inttypes.h>
#include <limits.h>
#include <stdio.h>
#include <stdint.h>
#include <stdlib.h>
#include <string.h>

static int line_is_complete(const char line[])
{
    return strchr(line, '\n') != NULL || feof(stdin);
}

static int trailing_space_only(char *end)
{
    while (isspace((unsigned char)*end)) {
        ++end;
    }
    return *end == '\0';
}

static int read_count(size_t *result)
{
    char line[128];
    if (fgets(line, sizeof line, stdin) == NULL || !line_is_complete(line)) {
        return 0;
    }

    char *start = line;
    while (isspace((unsigned char)*start)) {
        ++start;
    }
    if (*start == '-' || *start == '\0') {
        return 0;
    }

    errno = 0;
    char *end;
    uintmax_t value = strtoumax(start, &end, 10);
    if (end == start || errno == ERANGE || value == 0 ||
        value > SIZE_MAX || !trailing_space_only(end)) {
        return 0;
    }
    *result = (size_t)value;
    return 1;
}

static int read_int(int *result)
{
    char line[128];
    if (fgets(line, sizeof line, stdin) == NULL || !line_is_complete(line)) {
        return 0;
    }

    errno = 0;
    char *end;
    intmax_t value = strtoimax(line, &end, 10);
    if (end == line || errno == ERANGE || value < INT_MIN ||
        value > INT_MAX || !trailing_space_only(end)) {
        return 0;
    }
    *result = (int)value;
    return 1;
}

int main(void)
{
    size_t count;
    if (!read_count(&count) || count > SIZE_MAX / sizeof(int)) {
        return 1;
    }

    int *values = malloc(count * sizeof *values);
    if (values == NULL) {
        return 1;
    }
    for (size_t i = 0; i < count; ++i) {
        if (!read_int(&values[i])) {
            free(values);
            return 1;
        }
    }

    int minimum = values[0];
    int maximum = values[0];
    double total = 0.0;
    for (size_t i = 0; i < count; ++i) {
        if (values[i] < minimum) minimum = values[i];
        if (values[i] > maximum) maximum = values[i];
        total += values[i];
    }
    printf("%.2f %d %d\n", total / (double)count, minimum, maximum);
    free(values);
    return 0;
}
```

## 6. 코드 해석
각 값은 한 줄씩 bounded buffer에 읽힌다. 변환 실패·range 초과·음수 count·trailing junk를 allocation 또는 저장 전에 거부한다. zero count를 거부해 `values[0]`을 valid하게 만들며 element input 실패 경로도 storage를 해제한다.

## 7. 내부 동작
input parsing, allocation, computation, cleanup은 서로 다른 책임이다. allocator는 element count나 입력 validity를 저장해 주지 않는다.

## 8. 자주 하는 실수
- `scanf` 반환값만 확인하면 모든 out-of-range 정수 입력이 안전하다고 생각한다.
- `strtoumax`가 leading minus sign도 원하는 정책대로 거부한다고 가정한다.
- zero count에서 `values[0]`을 읽는다.
- element input 실패 시 leak을 만든다.

## 9. 필수 실습
양의 count와 정수들을 입력받아 평균·최솟값·최댓값을 출력한다.
[18-18 exercise](../../exercises/18-dynamic-memory/18-18/README.md)

## 10. 추가 실습
- ★ 합계도 출력한다.
- ★★ 통계 계산을 읽기 전용 함수로 분리한다.
- ★★★ 여러 값을 한 줄에서 안전하게 분리하는 parser 설계를 조사한다.

## 11. 확인 문제
1. `scanf` 정수 변환의 반환값 검사만으로 range 안전성을 보장할 수 있는가?
2. count가 0이면 왜 문제인가?
3. 음수 count를 변환 전에 명시적으로 거부하는 이유는?
4. pointer만으로 count를 알아낼 수 있는가?
5. 평균 계산에서 cast가 필요한 이유는?

## 12. 핵심 정리
- 입력 경계에서 count와 elements를 검증한다.
- pointer와 count를 함께 전달한다.
- 모든 exit path에서 owned allocation을 정리한다.

## 13. 다음 Step
[18-19. 동적 배열 정렬](18-19-dynamic-array-sorting.md)

## 14. 참고 자료
- N1570 7.21.6.2 `fscanf`; 7.22.1.4 `strtol`; 7.8.2.3 `strtoimax`; 7.22.3.4 `malloc`. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: `strtoimax`, `strtoumax`](https://en.cppreference.com/w/c/string/byte/strtoimax)
- [cppreference: malloc](https://en.cppreference.com/w/c/memory/malloc)
