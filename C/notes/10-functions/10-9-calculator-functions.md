# 10-9. 계산기 연산 함수 분리

계산기 연산을 함수로 분리하면 각 함수가 하나의 계산 규칙만 담당하고 `main`은 선택과 출력 흐름에 집중할 수 있다.

## 1. 학습 목표
- 사칙연산을 역할별 함수로 분리한다.
- 함수의 입력·결과 계약을 확인한다.
- 나눗셈의 0 경계를 호출 전에 검사한다.

## 2. 선수 지식
Part 5의 계산기 입력·출력, Part 7의 산술 연산, Step 10-1~10-4의 함수 문법을 사용한다.

## 3. 핵심 개념
`add`, `subtract`, `multiply`, `divide`처럼 한 함수가 한 연산을 담당하면 계산 규칙과 메뉴 선택을 분리할 수 있다. 함수 이름과 return type, parameter type은 연산 의미를 드러내야 한다. 나눗셈 함수의 제수가 0이 되지 않도록 caller가 경계를 확인하거나 함수 계약을 별도로 정해야 한다.

## 4. 문법
```c
double add(double left, double right);
double subtract(double left, double right);
double multiply(double left, double right);
double divide(double left, double right);
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

double add(double left, double right)
{
    return left + right;
}

double subtract(double left, double right)
{
    return left - right;
}

int main(void)
{
    double left = 8.0;
    double right = 3.0;
    printf("%.1f\n", add(left, right));
    printf("%.1f\n", subtract(left, right));
    return 0;
}
```

## 6. 코드 해석
두 functions는 같은 parameter 구조를 사용하지만 body의 연산이 다르다. `main`은 8.0과 3.0을 arguments로 전달해 덧셈 11.0과 뺄셈 5.0을 출력한다.

## 7. 내부 동작
[C17 표준] `double` arguments는 `double` parameters를 초기화하고 결과도 `double`이다. 부동소수점 결과의 표현과 rounding은 구현의 floating-point 특성과 연산에 영향을 받는다. [설계 관점] 함수 분리는 동작을 바꾸지 않으며 각 연산의 책임을 명확히 한다.

## 8. 자주 하는 실수
- 입력·메뉴·모든 계산·출력을 한 함수에 다시 몰아넣는다.
- 정수 나눗셈과 실수 나눗셈을 혼동한다.
- 제수 0 검사를 하지 않는다.
- 뺄셈과 나눗셈에서 argument 순서를 바꾼다.

## 9. 필수 실습
덧셈·뺄셈·곱셈 함수를 각각 정의하고 같은 두 값으로 호출한다. [실습 README](../../exercises/10-functions/10-9/README.md)

## 10. 추가 실습
- ★ 세 연산의 결과 출력
- ★★ 0이 아닌 제수를 확인한 실수 나눗셈 함수 호출
- ★★★ `switch`가 연산 함수 결과를 선택하도록 구성

## 11. 확인 문제
1. 연산별 함수 분리의 직접적인 장점은?
2. 뺄셈 argument 순서가 중요한 이유는?
3. 나눗셈 전에 확인할 경계는?
4. `double` 함수 호출 결과의 타입은?
5. 함수 분리가 C 연산 규칙을 바꾸는가?

## 12. 핵심 정리
계산 함수는 한 연산만 담당하고 caller는 입력 검증·연산 선택·출력을 조정한다.

## 13. 다음 Step
[Step 10-10. `print_menu`와 입력·처리·출력 분리](10-10-menu-separation.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.2.2, 6.5.5, 6.9.1
- [cppreference: Function call](https://en.cppreference.com/w/c/language/operator_other.html#Function_call)
- [cppreference: Arithmetic operators](https://en.cppreference.com/w/c/language/operator_arithmetic.html)
