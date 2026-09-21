# 17-7. 읽기 전용 배열 매개변수

함수가 배열 원소를 읽기만 한다면 `const`가 붙은 array parameter 표기로 수정하지 않겠다는 API 의도를 표현할 수 있다.

## 1. 학습 목표
- `const int values[]` parameter와 pointer adjustment를 함께 설명한다.
- pointer value pass-by-value와 element access를 구분한다.
- 읽기 전용 함수 interface를 작성한다.

## 2. 선수 지식
Part 16의 array parameter adjustment·count parameter와 Step 17-2의 pointer to const를 안다.

## 3. 핵심 개념

```c
int sum(const int values[], size_t count);
```

function parameter declarator에서 `const int values[]`는 조정되어 `const int *values`에 해당한다. 배열 전체가 함수로 복사되는 것도 아니고, caller array가 새 const object로 바뀌는 것도 아니다.

pointer value는 pass-by-value로 parameter object에 전달된다. 함수 안에서는 `values[i]`를 읽을 수 있지만 이 parameter expression을 통해 element를 수정할 수 없다.

## 4. 문법

```c
int sum(const int values[], size_t count);
int sum(const int *values, size_t count);
```

이 parameter 문맥에서는 두 선언이 조정 후 같은 parameter type을 나타낸다. 배열 원소 수 정보는 함께 전달되지 않으므로 `count`가 필요하다.

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
    int scores[] = {10, 20, 30};

    printf("sum = %d\n", sum(scores, 3));
    return 0;
}
```

## 6. 코드 해석
1. `scores` argument는 첫 원소를 가리키는 pointer value로 변환된다.
2. 그 pointer value의 copy가 `values` parameter에 전달된다.
3. `const int` pointed-to type 때문에 `values[i]`를 통해 element를 수정할 수 없다.
4. `count`가 유효한 element 범위를 알려 준다.

## 7. 내부 동작
- **[C17 표준]** function parameter의 array declarator는 qualified element type을 가리키는 pointer parameter로 조정된다.
- **[C17 표준]** pointer argument도 pass-by-value다.
- **[구현]** compiler는 `const`를 API type checking에 사용하지만 항상 더 빠른 machine code를 생성한다고 보장하지 않는다.

`sizeof(values)`는 caller array 크기가 아니라 조정된 pointer parameter의 크기를 계산한다. 배열 길이는 `count`로 관리한다.

## 8. 자주 하는 실수
- 함수가 const 배열을 값으로 복사받는다고 설명한다.
- `const` parameter가 caller array 자체를 const object로 바꾼다고 생각한다.
- `sizeof(values) / sizeof(values[0])`로 element 수를 구하려 한다.
- pointer parameter 전달을 call by reference라고 부른다.

## 9. 필수 실습
`average(const int values[], size_t count)`를 작성한다. `count > 0`인 입력만 사용하고 element를 수정하지 않은 채 평균을 반환한다.

실습 안내: [17-7 exercise](../../exercises/17-const-and-pointers/17-7/README.md)

## 10. 추가 실습
- ★ 기초: 배열의 최댓값을 읽기만 하는 함수를 작성한다.
- ★★ 응용: `const int *values` 표기로 바꿔 같은 결과를 확인한다.
- ★★★ 도전: 두 배열을 읽기만 해 같은 위치의 원소 차이를 출력하는 함수를 작성한다.

## 11. 확인 문제
1. parameter의 `const int values[]`는 어떤 pointer type으로 조정되는가?
2. 배열 전체가 함수로 복사되는가?
3. caller의 non-const 배열이 const object로 바뀌는가?
4. element 수를 별도 전달해야 하는 이유는 무엇인가?
5. `sizeof(values)`가 caller array 크기를 주지 않는 이유는 무엇인가?

## 12. 핵심 정리
- 읽기 전용 array parameter는 `const`로 함수 의도를 표현한다.
- array parameter adjustment 뒤에는 pointer to const가 된다.
- pointer value는 pass-by-value로 전달된다.
- 배열 길이는 별도의 count parameter가 담당한다.

## 13. 다음 Step
[17-8. 문자열 리터럴과 `const char *`](17-8-string-literals-and-const-char-pointer.md)

## 14. 참고 자료
- ISO/IEC 9899:2011 Committee Draft N1570, 6.7.6.3 Function declarators, paragraph 7. N1570은 **C11 공개 Committee Draft**이며 array parameter adjustment 규칙은 C17에서도 유지된다.
- [cppreference: array declarations](https://en.cppreference.com/w/c/language/array)
- [cppreference: const type qualifier](https://en.cppreference.com/w/c/language/const)
