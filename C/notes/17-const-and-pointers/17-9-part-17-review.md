# 17-9. Part 17 종합 복습

Part 17에서는 `const`가 어느 타입 층을 한정하는지 읽고, pointer·array parameter·string interface에서 수정 의도를 정확히 표현하는 방법을 학습했다.

## 1. 학습 목표
- const object와 modifiable lvalue를 설명한다.
- 네 가지 pointer/const 선언을 정확히 비교한다.
- qualification conversion과 읽기 전용 함수 interface를 종합한다.

## 2. 선수 지식
Step 17-1부터 17-8까지와 Part 14~16의 pointer·array parameter 규칙을 사용한다.

## 3. 핵심 개념

| 선언 | `p` 재지정 | `*p`를 통한 수정 |
|---|---:|---:|
| `int *p` | 가능 | 가능 |
| `const int *p` | 가능 | 불가 |
| `int *const p = &value` | 불가 | 가능 |
| `const int *const p = &value` | 불가 | 불가 |

`const`는 compile-time constant나 특정 memory section을 뜻하지 않는다. pointer to const는 해당 접근 경로의 수정을 제한하며 object 자체의 선언을 바꾸지 않는다. 함수의 읽기 전용 array parameter는 pointer adjustment 뒤 pointer to const parameter가 된다.

## 4. 문법

```c
const int object = 10;
const int *read_only_path = &object;

int value = 20;
int *const fixed_path = &value;
const int *const fixed_read_only_path = &value;

int sum(const int values[], size_t count);
void print_text(const char text[]);
```

## 5. 최소 코드 예제

```c
#include <stddef.h>
#include <stdio.h>

static int sum(const int values[], size_t count)
{
    int total = 0;

    for (size_t i = 0; i < count; ++i) {
        total += values[i];
    }
    return total;
}

int main(void)
{
    int values[] = {2, 4, 6};
    int *const fixed_path = &values[0];
    const int *read_only_path = values;

    *fixed_path = 8;
    printf("%d %d\n", *read_only_path, sum(values, 3));
    return 0;
}
```

## 6. 코드 해석
1. `fixed_path`의 pointer value는 고정되지만 첫 element는 수정할 수 있다.
2. `read_only_path`는 재지정 가능하지만 이를 통한 element 수정은 허용되지 않는다.
3. `sum`은 adjusted pointer parameter로 elements를 읽고 count로 경계를 관리한다.
4. 출력은 수정된 첫 element와 전체 합을 보여 준다.

## 7. 내부 동작
- **[C17 표준]** const qualification, modifiable lvalue, pointer assignment constraint, array parameter adjustment가 허용 연산을 결정한다.
- **[GCC 구현]** constraint 위반을 diagnostic으로 보여 주며 최적화와 object 배치는 compiler가 결정한다.
- **[ABI / OS / CPU 구현]** read-only section이나 page protection은 환경의 구현 세부사항이다.

compile 성공은 ISO C17의 모든 semantic guarantee를 자동 증명하지 않는다. 표준 규칙과 compiler 관찰을 함께 사용하되 둘을 같은 근거로 취급하지 않는다.

## 8. 자주 하는 실수
- `const int`를 C의 모든 constant expression 문맥에 쓸 수 있다고 생각한다.
- pointer to const와 const pointer를 뒤바꾼다.
- pointer to const가 실제 object를 const object로 바꾼다고 생각한다.
- const array parameter를 배열 pass-by-value라고 설명한다.
- C string literal을 C++처럼 `const char[N]`이라고 일반화한다.
- `const`가 항상 ROM 배치나 성능 향상을 보장한다고 생각한다.

## 9. 필수 실습
네 pointer 형태의 허용 연산 표를 작성하고, `sum(const int values[], size_t count)`와 `print_text(const char text[])`를 포함한 종합 프로그램을 만든다.

실습 안내: [17-9 exercise](../../exercises/17-const-and-pointers/17-9/README.md)

## 10. 추가 실습
- ★ 기초: 각 선언을 자연어로 읽는다.
- ★★ 응용: non-const object를 수정 가능한 경로와 읽기 전용 경로로 함께 관찰한다.
- ★★★ 도전: 허용되는 qualification 추가와 금지되는 제거를 코드 실행 없이 분석표로 정리한다.

## 11. 확인 문제
1. 타입이 const-qualified인 lvalue가 modifiable lvalue인가? const-qualified type으로 정의된 object를 cast로 얻은 non-const lvalue를 통해 수정하려 하면 어떻게 되는가?
2. `const int *p`와 `int *const p`의 차이는 무엇인가?
3. `const int *const p`에서 두 `const`는 각각 무엇을 한정하는가?
4. qualification 추가가 실제 object의 선언을 바꾸는가?
5. `const int values[]` parameter가 배열 전체를 복사받는가?
6. C17 string literal 수정이 왜 실행 실습에 부적절한가?
7. `const`만으로 memory placement나 성능을 보장할 수 있는가?

## 12. 핵심 정리
- const qualification이 적용되는 타입 층을 먼저 찾는다.
- `p` 재지정과 `*p` 수정 가능 여부를 따로 판단한다.
- qualifier 추가와 제거 방향을 구분한다.
- 읽기 전용 parameter는 API 의도를 표현하지만 pass-by-value 규칙은 그대로다.
- C17 규칙과 compiler·ABI·OS 구현을 구분한다.

## 13. 다음 Step
커리큘럼의 다음 Step은 **18-1. stack·heap 모델과 C의 storage duration**이다. Part 18 파일은 이 Part에서 만들지 않는다.

## 14. 참고 자료
- ISO/IEC 9899:2011 Committee Draft N1570, 6.3.2.1; 6.3.2.3; 6.4.5; 6.5.16.1; 6.7.3; 6.7.6.3. N1570은 **C11 공개 Committee Draft**이며 인용한 관련 규칙은 C17에서도 유지된다.
- [cppreference: const type qualifier](https://en.cppreference.com/w/c/language/const)
- [cppreference: pointer declarations](https://en.cppreference.com/w/c/language/pointer)
- [GCC: C Dialect Options](https://gcc.gnu.org/onlinedocs/gcc/C-Dialect-Options.html)
