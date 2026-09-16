# 10-1. 함수 정의와 호출

함수는 이름이 붙은 작업 단위이며, 호출식으로 제어를 함수 본문에 전달하고 결과를 받을 수 있다.

## 1. 학습 목표
- 함수 정의의 반환형·이름·parameter list·본문을 구분한다.
- 함수 호출의 argument와 반환값을 찾는다.
- `main`도 함수 정의임을 설명한다.

## 2. 선수 지식
Part 1의 `int main(void)`, Part 6의 형 변환, Part 8~9의 제어 흐름을 사용한다.

## 3. 핵심 개념
함수 정의는 함수가 수행할 동작을 제공한다. `int add(int a, int b)`에서 `int`는 return type, `add`는 function name, `(int a, int b)`는 parameter list, 중괄호 안은 function body다. `return a + b;`는 계산한 값을 caller로 돌려준다. `add(3, 4)`는 function call이고 3과 4는 arguments다.

## 4. 문법
```c
return_type function_name(parameter_list)
{
    statements;
    return expression;
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int add(int a, int b)
{
    return a + b;
}

int main(void)
{
    int result = add(3, 4);
    printf("%d\n", result);
    return 0;
}
```

## 6. 코드 해석
`main`이 `add`를 3과 4로 호출한다. parameter `a`, `b`는 각각 전달된 값으로 초기화되고 합 7이 반환된다. 호출식의 값 7이 `result`를 초기화하며 7을 출력한다.

## 7. 내부 동작
[C 언어 관점] 호출식은 함수 본문 실행과 반환을 포함하는 expression이다. 실행은 called function으로 넘어갔다가 `return` 후 caller의 호출식 다음 흐름으로 돌아온다. [컴파일러/ABI 관점] argument가 register나 memory 중 어디로 전달되는지는 C17이 아니라 구현과 ABI가 정한다.

## 8. 자주 하는 실수
- definition, call, return statement를 같은 개념으로 부른다.
- parameter와 argument를 구분하지 않는다.
- 함수 정의 끝에 불필요한 세미콜론을 붙인다.
- 모든 argument가 반드시 stack으로 전달된다고 단정한다.

## 9. 필수 실습
두 정수를 더해 반환하는 `add`를 정의하고 `main`에서 호출한다. [실습 README](../../exercises/10-functions/10-1/README.md)

## 10. 추가 실습
- ★ 두 수의 곱을 반환하는 함수
- ★★ 세 수의 합을 반환하는 함수
- ★★★ `main`과 일반 함수 정의의 공통 요소 표시

## 11. 확인 문제
1. `int add(int a, int b)`의 return type은 무엇인가?
2. `a`, `b`는 parameter인가 argument인가?
3. `add(3, 4)`의 3과 4는 무엇인가?
4. 호출 결과 7은 어디에 저장되는가?
5. C17이 argument의 register 전달을 보장하는가?

## 12. 핵심 정리
함수 정의는 작업을 제공하고 호출은 arguments를 전달하며, `return` 값은 호출식의 결과가 된다.

## 13. 다음 Step
[Step 10-2. 함수 선언과 prototype](10-2-declaration-prototype.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.2.2, 6.9.1
- [cppreference: Function declaration](https://en.cppreference.com/w/c/language/function_declaration.html)
- [cppreference: Function definition](https://en.cppreference.com/w/c/language/function_definition.html)
