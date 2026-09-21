# 17-2. `const int *p`

`const int *p`는 `p`가 가리키는 `int`를 **`p`를 통해 수정하지 않겠다**는 타입 관계를 표현한다.

## 1. 학습 목표
- `const int *p`를 pointer to const int로 읽는다.
- pointer 재지정과 `*p`를 통한 수정을 구분한다.
- pointer to const가 원래 객체 자체를 const 객체로 바꾸지 않음을 설명한다.

## 2. 선수 지식
Step 17-1의 const-qualified type과 Part 14의 pointer·dereference를 안다.

## 3. 핵심 개념

```c
int first = 10;
int second = 20;
const int *p = &first;
```

`p` 자체는 non-const pointer object이므로 `p = &second;`처럼 재지정할 수 있다. 그러나 `*p` expression의 타입은 `const int`이므로 `*p = 30;`은 허용되지 않는다.

`first`는 여전히 non-const `int` 객체다. 따라서 `first = 30;`처럼 다른 modifiable lvalue를 통한 수정은 가능하다. `p`는 읽기 전용 **접근 경로**를 제공할 뿐 객체의 선언된 타입을 바꾸지 않는다.

## 4. 문법

```c
const int *p;
int const *q;
```

두 선언 모두 pointer to const int를 나타낸다. `const`가 `int`를 한정하고 pointer object 자체는 한정하지 않는다.

## 5. 최소 코드 예제

```c
#include <stdio.h>

int main(void)
{
    int first = 10;
    int second = 20;
    const int *p = &first;

    printf("%d\n", *p);
    p = &second;
    printf("%d\n", *p);
    first = 30;
    printf("%d\n", first);
    return 0;
}
```

## 6. 코드 해석
1. non-const `first`의 주소를 pointer to const인 `p`에 저장한다.
2. `*p`를 읽는 것은 허용된다.
3. `p`를 `second`의 주소로 재지정하는 것도 허용된다.
4. `first` 객체는 const 객체가 아니므로 `first`를 직접 수정할 수 있다.

## 7. 내부 동작

```text
first
+------+
|  10  |
+------+
   ^
   | p를 통한 접근에서는 읽기 전용
   |
p
+----------------+
| pointer value  |
+----------------+
```

여기서 "읽기 전용"은 `p` expression을 통한 type-system상의 제한이다. `first`가 물리적 read-only memory로 이동했다는 뜻이 아니다. machine code와 배치는 compiler·ABI·OS가 결정한다.

## 8. 자주 하는 실수
- `const int *p`에서 `p` 자체도 재지정할 수 없다고 생각한다.
- non-const 객체를 가리키면 `const`가 사라진다고 생각한다.
- `p`가 가리키는 순간 원래 객체가 const 객체로 변한다고 생각한다.
- `int *p = &const_object;`처럼 qualifier를 제거하는 방향을 정상 초기화로 사용한다.

## 9. 필수 실습
두 `int` 객체를 만들고 하나의 `const int *`가 차례로 두 객체를 가리키며 값을 읽게 한다. `*p`를 통한 assignment는 작성하지 않는다.

실습 안내: [17-2 exercise](../../exercises/17-const-and-pointers/17-2/README.md)

## 10. 추가 실습
- ★ 기초: `int const *` 표기로 같은 프로그램을 작성한다.
- ★★ 응용: 원래 이름으로 객체를 수정한 뒤 `*p`로 변경된 값을 읽는다.
- ★★★ 도전: `*p = 1;`을 별도 파일에서 compile만 해 diagnostic을 관찰하고 즉시 원상 복구한다.

## 11. 확인 문제
1. `const int *p`에서 무엇이 const-qualified인가?
2. `p = &second;`가 가능한 이유는 무엇인가?
3. `*p = 20;`이 허용되지 않는 이유는 무엇인가?
4. `int value; const int *p = &value;`가 `value` 자체를 const 객체로 바꾸는가?
5. `const int *p`와 `int const *p`의 pointed-to type은 같은가?

## 12. 핵심 정리
- `const int *p`는 pointer to const int다.
- `p`는 재지정할 수 있지만 `*p`를 통해 값을 수정할 수 없다.
- non-const 객체도 pointer to const로 읽을 수 있다.
- 접근 경로의 qualifier와 객체 자체의 선언을 구분한다.

## 13. 다음 Step
[17-3. `int *const p`](17-3-const-pointer.md)

## 14. 참고 자료
- ISO/IEC 9899:2011 Committee Draft N1570, 6.2.5 Types; 6.5.16.1 Simple assignment; 6.7.3 Type qualifiers. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: const type qualifier](https://en.cppreference.com/w/c/language/const)
- [cppreference: pointer declarations](https://en.cppreference.com/w/c/language/pointer)
