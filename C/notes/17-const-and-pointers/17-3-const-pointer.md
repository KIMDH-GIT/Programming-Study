# 17-3. `int *const p`

`int *const p`는 pointer object `p` 자체가 const-qualified이고, pointed-to `int`는 const-qualified가 아닌 선언이다.

## 1. 학습 목표
- `int *const p`를 const pointer to int로 읽는다.
- `p` 재지정 금지와 `*p` 수정 가능을 분리한다.
- const pointer를 initializer와 함께 선언한다.

## 2. 선수 지식
Step 17-2의 pointer to const와 pointer object·pointed-to object의 차이를 안다.

## 3. 핵심 개념

```c
int value = 10;
int *const p = &value;
```

`const`는 `p`라는 pointer object를 한정한다. 따라서 `p = &another;`처럼 저장된 pointer value를 바꾸는 assignment는 허용되지 않는다. 반면 `*p`의 타입은 non-const `int`이므로 valid lifetime 안의 `value`를 `*p = 20;`으로 수정할 수 있다.

## 4. 문법

```c
pointed_type *const identifier = valid_address;
```

const-qualified pointer object는 이후 재지정할 수 없으므로 선언할 때 사용할 주소로 초기화하는 형태가 가장 명확하다.

## 5. 최소 코드 예제

```c
#include <stdio.h>

int main(void)
{
    int value = 10;
    int *const p = &value;

    *p = 20;
    printf("value = %d\n", value);
    return 0;
}
```

## 6. 코드 해석
1. `p`는 `value`의 주소로 초기화된 const pointer object다.
2. `p`에 저장된 pointer value는 assignment로 바꿀 수 없다.
3. dereference 결과 `*p`는 non-const `int` 객체 `value`를 지정한다.
4. `*p = 20;`은 `value`를 정상적으로 수정한다.

## 7. 내부 동작

```text
value
+------+
|  10  |  <- *p로 수정 가능
+------+
   ^
   |
p [저장된 pointer value 재지정 불가]
```

const가 붙은 대상은 pointer object `p`다. 주소가 가리키는 `value`의 타입이나 memory protection을 바꾸지 않는다.

## 8. 자주 하는 실수
- `int *const p`를 pointer to const int로 읽는다.
- `p`가 const이므로 `*p`도 수정할 수 없다고 생각한다.
- initializer 없이 선언한 뒤 나중에 주소를 대입하려 한다.
- const pointer가 가리키는 object lifetime도 자동으로 연장된다고 생각한다.

## 9. 필수 실습
하나의 `int` 객체와 그 주소로 초기화한 `int *const`를 만들고, `*p`로 값을 두 번 수정해 출력한다.

실습 안내: [17-3 exercise](../../exercises/17-const-and-pointers/17-3/README.md)

## 10. 추가 실습
- ★ 기초: `*p += 5;`로 pointed-to object를 수정한다.
- ★★ 응용: `p`와 `&value`를 `%p`로 출력해 같은 pointer value인지 확인한다.
- ★★★ 도전: `p = &other;`를 별도 compile 분석 대상으로 만들고 diagnostic의 원인이 pointer object의 const qualification임을 적는다.

## 11. 확인 문제
1. `int *const p`에서 const-qualified인 대상은 무엇인가?
2. `p = &other;`가 허용되지 않는 이유는 무엇인가?
3. `*p = 30;`은 왜 가능한가?
4. const pointer가 pointed-to object의 lifetime을 연장하는가?
5. 이 선언에 initializer를 쓰는 것이 중요한 이유는 무엇인가?

## 12. 핵심 정리
- `int *const p`는 const pointer to int다.
- `p`는 재지정할 수 없다.
- pointed-to `int`가 non-const이면 `*p`로 수정할 수 있다.
- pointer의 qualification과 pointed-to type의 qualification은 별개다.

## 13. 다음 Step
[17-4. `const int *const p`](17-4-const-pointer-to-const.md)

## 14. 참고 자료
- ISO/IEC 9899:2011 Committee Draft N1570, 6.7.3 Type qualifiers; 6.7.6 Declarators. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: const type qualifier](https://en.cppreference.com/w/c/language/const)
- [cppreference: pointer declarations](https://en.cppreference.com/w/c/language/pointer)
