# 17-4. `const int *const p`

`const int *const p`는 pointer object와 pointed-to type 양쪽에 각각 `const`가 적용된 선언이다.

## 1. 학습 목표
- `const int *const p`를 const pointer to const int로 읽는다.
- pointer 재지정과 pointed-to object 수정이 각각 제한됨을 설명한다.
- 네 가지 pointer/const 조합을 비교한다.

## 2. 선수 지식
Step 17-2의 pointer to const와 Step 17-3의 const pointer를 안다.

## 3. 핵심 개념

```c
const int value = 10;
const int *const p = &value;
```

identifier 가까이의 `const`는 pointer object `p`를 한정한다. `*` 왼쪽의 `const int`는 pointed-to type을 한정한다. 따라서 `p`를 다른 주소로 재지정할 수도 없고 `*p`를 통해 값을 수정할 수도 없다.

## 4. 문법

| 선언 | `p` 재지정 | `*p`를 통한 수정 |
|---|---:|---:|
| `int *p` | 가능 | 가능 |
| `const int *p` | 가능 | 불가 |
| `int *const p = &value` | 불가 | 가능 |
| `const int *const p = &value` | 불가 | 불가 |

표의 수정 가능 여부는 valid pointer와 object lifetime을 전제로 하며, **해당 pointer expression을 통해** 가능한지를 뜻한다.

## 5. 최소 코드 예제

```c
#include <stdio.h>

int main(void)
{
    const int value = 10;
    const int *const p = &value;

    printf("value = %d\n", *p);
    return 0;
}
```

## 6. 코드 해석
1. `value`는 const-qualified `int` 객체다.
2. `p`는 `value`를 가리키도록 초기화된 const pointer다.
3. `p`의 pointer value는 재지정할 수 없다.
4. `*p`는 const-qualified `int`를 지정하므로 수정할 수 없다.

## 7. 내부 동작
두 제한은 서로 다른 타입 층에 있다. pointer object에는 주소를 나타내는 pointer value가 저장되고, dereference expression은 pointed-to object를 지정한다. `const`는 각 층의 modifiable lvalue 여부에 따로 영향을 준다. 실제 memory section이나 CPU 보호 속성은 별개의 구현 문제다.

## 8. 자주 하는 실수
- 두 `const`가 같은 대상을 두 번 한정한다고 생각한다.
- pointer 자체의 const와 pointed-to type의 const를 표에서 뒤바꾼다.
- const pointer이면 invalid pointer도 안전하다고 생각한다.
- 읽기만 하므로 object lifetime을 확인하지 않아도 된다고 생각한다.

## 9. 필수 실습
네 가지 선언을 표로 다시 작성하고, 각 선언에서 `p` 재지정과 `*p` 수정 가능 여부를 표시한다. 그중 `const int *const`의 읽기 예제를 실행한다.

실습 안내: [17-4 exercise](../../exercises/17-const-and-pointers/17-4/README.md)

## 10. 추가 실습
- ★ 기초: 선언마다 const가 한정하는 대상을 밑줄로 표시한다.
- ★★ 응용: non-const 객체를 `const int *const`로 읽되 객체 자체는 다른 이름으로 수정 가능한 이유를 설명한다.
- ★★★ 도전: 네 선언의 허용·금지 assignment를 실행하지 않는 분석표로 만든다.

## 11. 확인 문제
1. 첫 번째 `const`와 두 번째 `const`는 각각 무엇을 한정하는가?
2. `p = &other;`가 허용되는가?
3. `*p = 20;`이 허용되는가?
4. `const int *const p`가 valid lifetime을 보장하는가?
5. `int *const p`와 비교했을 때 추가로 제한되는 동작은 무엇인가?

## 12. 핵심 정리
- `const int *const p`는 const pointer to const int다.
- pointer 재지정과 `*p`를 통한 수정이 모두 제한된다.
- 두 `const`는 서로 다른 타입 층을 한정한다.
- const qualification은 pointer validity나 lifetime 검사를 대신하지 않는다.

## 13. 다음 Step
[17-5. 선언을 오른쪽에서 왼쪽으로 읽기](17-5-reading-declarations.md)

## 14. 참고 자료
- ISO/IEC 9899:2011 Committee Draft N1570, 6.2.5 Types; 6.7.3 Type qualifiers; 6.7.6 Declarators. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: const type qualifier](https://en.cppreference.com/w/c/language/const)
- [cppreference: pointer declarations](https://en.cppreference.com/w/c/language/pointer)
