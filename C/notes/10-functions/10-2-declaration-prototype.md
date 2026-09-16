# 10-2. 함수 선언과 prototype

함수 선언은 호출 전에 함수의 이름과 타입 정보를 compiler에게 알리며, prototype은 parameter 타입까지 명시한다.

## 1. 학습 목표
- declaration, prototype, definition, call을 구분한다.
- 호출보다 정의가 뒤에 있을 때 필요한 선언을 작성한다.
- C17에서 `f(void)`와 `f()`의 차이를 설명한다.

## 2. 선수 지식
Step 10-1의 함수 정의와 호출을 안다.

## 3. 핵심 개념
`int add(int, int);`는 함수 declaration이자 prototype이다. 함수 본문은 없고 타입 정보만 제공한다. `int add(int a, int b) { ... }`는 definition이며 동시에 declaration 역할도 한다. C17에서 `int f(void);`는 parameter가 없음을 명시하는 prototype이지만, `int f();`는 parameter 수와 타입을 지정하지 않는 old-style declaration이며 둘은 동일하지 않다.

## 4. 문법
```c
int add(int, int);          /* declaration and prototype */
int no_parameter(void);     /* no parameters */
int unspecified();          /* not a prototype in C17 */
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int add(int, int);

int main(void)
{
    printf("%d\n", add(2, 5));
    return 0;
}

int add(int a, int b)
{
    return a + b;
}
```

## 6. 코드 해석
`main`보다 위의 prototype이 `add`의 return type과 두 parameter 타입을 먼저 알린다. 따라서 compiler는 호출의 argument 개수와 타입을 검사할 수 있다. 아래 definition이 실제 본문을 제공한다.

## 7. 내부 동작
[C17 표준] 함수 호출 지점에는 적절한 declaration이 보여야 한다. prototype이 보이면 argument는 대응 parameter type과 검사·변환된다. 선언 없는 호출을 과거 C의 implicit `int` 함수처럼 취급하는 규칙은 C17에 없다. 위반에는 diagnostic이 필요하며 portable C17 코드가 아니다.

## 8. 자주 하는 실수
- prototype 뒤의 세미콜론을 빼먹는다.
- prototype과 definition의 return type 또는 parameter type을 다르게 쓴다.
- C에서 `int f()`가 “인자 없음”을 뜻한다고 설명한다.
- GCC가 extension으로 번역할 수 있으면 C17에 맞는다고 생각한다.

## 9. 필수 실습
`main` 아래에 정의한 `subtract`를 위쪽 prototype을 통해 호출한다. [실습 README](../../exercises/10-functions/10-2/README.md)

## 10. 추가 실습
- ★ parameter 이름 없는 prototype 작성
- ★★ `f(void)`와 `f()` 의미 비교표
- ★★★ declaration을 제거한 코드를 실행하지 말고 diagnostic 이유 분석

## 11. 확인 문제
1. 모든 declaration이 definition인가?
2. prototype 끝에는 무엇이 필요한가?
3. C17에서 `int f(void);`는 무엇을 뜻하는가?
4. C17에서 `int f();`가 제공하지 않는 정보는?
5. 호출 전에 declaration이 필요한 이유는?

## 12. 핵심 정리
prototype은 호출 전 함수 타입을 제공하며, C17의 `(void)`와 빈 `()` 선언은 의미가 다르다.

## 13. 다음 Step
[Step 10-3. 매개변수와 인수](10-3-parameters-arguments.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.7.6.3, 6.5.2.2, 6.9.1
- [cppreference: Function declaration](https://en.cppreference.com/w/c/language/function_declaration.html)
- [GCC: Warning Options](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html)
