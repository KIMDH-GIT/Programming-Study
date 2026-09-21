# 17-6. const qualifier 추가·제거

object pointer를 대입하거나 초기화할 때 pointed-to type에 `const`를 추가하는 방향과 제거하는 방향은 같지 않다.

## 1. 학습 목표
- non-const object pointer에서 pointer to const로 가는 방향을 설명한다.
- qualifier를 제거하는 위험한 pointer assignment를 판별한다.
- const cast를 실행 실습으로 사용하지 않는다.

## 2. 선수 지식
Step 17-2의 pointer to const와 Part 14의 compatible pointer types를 안다.

## 3. 핵심 개념

```c
int value = 10;
int *p = &value;
const int *cp = p;
```

`cp`는 `value`를 읽을 수 있지만 `*cp`를 통해 수정할 수 없다. qualification을 추가한 접근 경로다. 원래 `p`와 `value`는 그대로이므로 `*p = 20;`은 가능하다.

반대 방향은 안전하지 않다.

```c
const int value = 10;
const int *cp = &value;
/* int *p = cp; */ /* pointed-to type의 const를 제거하는 제약 위반 */
```

compiler diagnostic의 근본 원인은 assignment operand의 pointer type이 C17 제약을 만족하지 않는다는 데 있다.

## 4. 문법

```c
int *modifiable_path = &value;
const int *read_only_path = modifiable_path; /* qualifier 추가 */
```

이 Step은 한 단계 object pointer만 다룬다. `int **`에서 `const int **`로의 변환은 단순히 같은 규칙을 반복한 것이 아니며 현재 범위에서 제외한다.

## 5. 최소 코드 예제

```c
#include <stdio.h>

int main(void)
{
    int value = 10;
    int *p = &value;
    const int *cp = p;

    *p = 20;
    printf("%d\n", *cp);
    return 0;
}
```

## 6. 코드 해석
1. `p`는 non-const `int`를 수정할 수 있는 접근 경로다.
2. `cp` 초기화에서는 pointed-to type에 `const`가 추가된다.
3. 두 pointer는 같은 object를 가리킨다.
4. `*p`로 수정한 결과를 `*cp`로 읽을 수 있다.

## 7. 내부 동작
- **[C17 표준]** simple assignment의 pointer operand에는 compatible type과 qualifier 관련 제약이 있다.
- **[GCC 구현]** qualifier 제거 방향을 옵션에 따라 warning 또는 error 형태로 진단할 수 있다.
- **[ABI / OS / CPU 구현]** pointer value의 bit pattern이 같을 수 있어도 C type 규칙상 허용되는 access는 다르다.

const-qualified type으로 정의된 object의 주소를 cast로 `int *`로 바꾼 뒤 실제로 수정하려 하면 undefined behavior다. 이 교재에서는 그런 코드를 실행하지 않는다.

## 8. 자주 하는 실수
- pointer value가 같은 주소이므로 qualifier도 무시할 수 있다고 생각한다.
- qualification 추가가 원래 object의 선언된 타입을 바꾼다고 생각한다.
- compiler warning만 없애려고 cast를 사용한다.
- `int **`에서 `const int **`도 항상 같은 방식으로 안전하다고 일반화한다.

## 9. 필수 실습
하나의 non-const `int`를 `int *`와 `const int *`가 함께 가리키게 한다. `int *`로 수정하고 `const int *`로 읽는다.

실습 안내: [17-6 exercise](../../exercises/17-const-and-pointers/17-6/README.md)

## 10. 추가 실습
- ★ 기초: qualification 추가 방향을 화살표로 표시한다.
- ★★ 응용: 허용되는 초기화와 제약을 위반하는 초기화를 표로 분류한다.
- ★★★ 도전: GCC diagnostic을 별도 분석 파일에서 관찰하되 cast나 실행은 하지 않는다.

## 11. 확인 문제
1. `int *`를 `const int *`에 저장하면 무엇이 추가되는가?
2. 이 변환이 실제 object를 const object로 바꾸는가?
3. `const int *`를 `int *`에 암시적으로 저장하면 왜 문제가 되는가?
4. cast가 const object 수정을 정의된 동작으로 바꾸는가?
5. `int **`와 `const int **` 변환을 이 Step 규칙으로 단순 일반화해도 되는가?

## 12. 핵심 정리
- 한 단계 object pointer에서는 pointed-to type에 const를 추가하는 방향을 사용할 수 있다.
- qualifier 제거 방향은 C17 assignment 제약을 위반할 수 있다.
- qualifier 추가는 object 자체의 선언을 바꾸지 않는다.
- const 제거 cast 뒤 const object를 수정하는 코드는 실행하지 않는다.

## 13. 다음 Step
[17-7. 읽기 전용 배열 매개변수](17-7-read-only-array-parameters.md)

## 14. 참고 자료
- ISO/IEC 9899:2011 Committee Draft N1570, 6.3.2.3 Pointers; 6.5.16.1 Simple assignment; 6.7.3 Type qualifiers. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: pointer declarations](https://en.cppreference.com/w/c/language/pointer)
- [GCC: Warning Options](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html)
