# 17-1. `const` 객체

`const`는 C의 type qualifier다. 객체를 지정하는 표현식의 타입이 `const`로 한정되면 그 표현식을 통해 객체를 수정할 수 없다.

## 1. 학습 목표
- `const int`를 C17의 type qualifier로 설명한다.
- const-qualified object와 modifiable lvalue를 구분한다.
- `const`와 compile-time constant, 저장 위치를 혼동하지 않는다.

## 2. 선수 지식
Part 7의 assignment와 lvalue, Part 14의 object·address 개념을 사용한다.

## 3. 핵심 개념

```c
const int value = 10;
```

`value`는 `const int` 타입의 객체다. `value` expression은 그 객체를 지정하는 lvalue지만 **modifiable lvalue는 아니다**. 그러므로 `value = 20;`은 assignment의 왼쪽 operand가 만족해야 하는 C17 제약을 위반한다.

`const`는 "모든 문맥에서 쓸 수 있는 compile-time constant"라는 뜻이 아니다. C17에서 block scope의 `const int n = 10;`은 일반적으로 integer constant expression이 아니므로 `case n:` 같은 문맥에 사용할 수 없다.

## 4. 문법

```c
const int value = 10;
int const other = 20;
```

두 선언은 모두 const-qualified `int` 객체를 만든다. 이 교재에서는 기본적으로 `const int` 순서를 사용한다.

## 5. 최소 코드 예제

```c
#include <stdio.h>

int main(void)
{
    const int value = 10;

    printf("value = %d\n", value);
    return 0;
}
```

## 6. 코드 해석
1. `value`는 `const int` 객체로 초기화된다.
2. 읽기는 허용되므로 `printf` argument로 값을 전달할 수 있다.
3. 예제에는 `value`를 수정하는 assignment가 없다.

## 7. 내부 동작
- **[C17 표준]** `const`는 타입의 의미를 한정한다. const-qualified type으로 정의된 객체를 수정하려는 동작은 허용되지 않는다.
- **[GCC 구현]** 잘못된 assignment에는 diagnostic을 출력한다. diagnostic 문구 자체는 GCC 구현의 표현이다.
- **[ABI / OS / CPU 구현]** 객체가 stack, register, read-only section 등 어디에 배치되는지는 선언 문맥·최적화·링커·운영체제에 달렸다. `const`가 곧 ROM이나 CPU write protection을 뜻하지 않는다.

## 8. 자주 하는 실수
- `const int`를 C++의 `constexpr`와 같다고 생각한다.
- `const` 객체는 언제나 `.rodata`에 있다고 단정한다.
- lvalue이면 항상 assignment 왼쪽에 올 수 있다고 생각한다. assignment에는 modifiable lvalue가 필요하다.
- `#define SIZE 10`과 `const int size = 10`을 같은 종류의 이름으로 생각한다. 전자는 전처리 macro이고 후자는 타입과 저장 공간을 가진 객체다.

## 9. 필수 실습
`const int temperature = 25;`를 선언하고 값을 출력한다. 수정 assignment 없이 읽기만 하는 프로그램을 작성한다.

실습 안내: [17-1 exercise](../../exercises/17-const-and-pointers/17-1/README.md)

## 10. 추가 실습
- ★ 기초: `const double pi = 3.14159;`를 출력한다.
- ★★ 응용: non-const 객체와 const 객체의 주소를 `%p`로 관찰하되 저장 영역을 단정하지 않는다.
- ★★★ 도전: `const int n = 3;`이 `case n:`에 쓰일 수 있는지 별도 파일에서 compile diagnostic만 관찰하고 실행하지 않는다.

## 11. 확인 문제
1. `const`는 C에서 어떤 종류의 문법 요소인가?
2. `value`가 lvalue이면서 modifiable lvalue가 아닐 수 있는 이유는 무엇인가?
3. `const int n = 10;`이 모든 문맥의 compile-time constant인가?
4. `const`만 보고 객체가 ROM에 있다고 말할 수 있는가?
5. compiler diagnostic과 C17 constraint는 어떻게 다른가?

## 12. 핵심 정리
- `const`는 type qualifier다.
- 타입이 const-qualified인 lvalue는 modifiable lvalue가 아니다.
- const-qualified type으로 정의된 object를 cast로 얻은 non-const lvalue를 통해 수정하려 해도 undefined behavior다.
- C의 `const int`는 C++의 `constexpr`가 아니다.
- 물리적 저장 위치와 보호 방식은 구현 문제다.

## 13. 다음 Step
[17-2. `const int *p`](17-2-pointer-to-const.md)

## 14. 참고 자료
- ISO/IEC 9899:2011 Committee Draft N1570, 6.3.2.1 Lvalues, arrays, and function designators; 6.7.3 Type qualifiers. N1570은 **C11 공개 Committee Draft**이며 여기의 관련 규칙은 C17에서도 유지된다.
- [cppreference: const type qualifier](https://en.cppreference.com/w/c/language/const)
- [GCC: Warning Options](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html)
