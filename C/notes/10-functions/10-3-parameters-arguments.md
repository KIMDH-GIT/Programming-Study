# 10-3. 매개변수와 인수

parameter는 함수 정의가 받을 값을 나타내는 객체이고, argument는 호출식에서 실제로 제공하는 expression이다.

## 1. 학습 목표
- parameter와 argument를 정확히 구분한다.
- 호출 시 parameter가 argument 값으로 초기화됨을 설명한다.
- parameter 순서와 타입을 확인한다.

## 2. 선수 지식
Step 10-2의 prototype과 Part 6의 conversion을 안다.

## 3. 핵심 개념
`int difference(int x, int y)`의 `x`, `y`는 parameters다. `difference(10, 3)`의 10과 3은 arguments다. 호출할 때 각 argument 값은 위치가 대응하는 parameter를 초기화한다. 이름이 같은 caller 변수와 parameter가 있어도 서로 다른 객체다.

## 4. 문법
```c
return_type function_name(type parameter, type parameter);
function_name(argument_expression, argument_expression);
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int difference(int x, int y)
{
    return x - y;
}

int main(void)
{
    int first = 10;
    int result = difference(first, 3);
    printf("%d\n", result);
    return 0;
}
```

## 6. 코드 해석
arguments는 `first` expression의 값 10과 정수 상수 3이다. parameters `x`, `y`가 각각 10과 3으로 초기화되어 7을 반환한다. `first`는 호출 뒤에도 10이다.

## 7. 내부 동작
[C17 표준] prototype이 보이는 호출에서는 각 argument가 대응 parameter type의 객체에 대입되는 것처럼 변환된다. argument expression의 평가 순서는 일반적으로 서로 지정되지 않으므로 여러 arguments에서 같은 객체를 위험하게 변경하지 않는다. [ABI 관점] 값의 물리적 전달 위치는 구현이 정한다.

## 8. 자주 하는 실수
- parameter와 argument를 같은 위치의 같은 객체라고 생각한다.
- argument 순서를 바꾸어도 뺄셈 결과가 같다고 생각한다.
- 함수 호출의 여러 arguments에서 같은 객체를 unsequenced하게 변경한다.
- parameter 이름이 prototype에도 반드시 필요하다고 생각한다.

## 9. 필수 실습
두 parameters를 받는 `larger` 함수를 만들고 두 arguments로 호출한다. [실습 README](../../exercises/10-functions/10-3/README.md)

## 10. 추가 실습
- ★ 두 수 중 작은 값 반환
- ★★ 세 arguments를 받는 합 함수
- ★★★ argument 순서를 바꾼 뺄셈 결과 비교

## 11. 확인 문제
1. definition의 `x`, `y`는 무엇인가?
2. 호출식의 `10`, `3`은 무엇인가?
3. parameter는 언제 초기화되는가?
4. prototype에서 parameter 이름을 생략할 수 있는가?
5. 여러 argument expression의 평가 순서를 임의로 가정해도 되는가?

## 12. 핵심 정리
arguments는 호출식의 값이고 parameters는 그 값으로 초기화되는 called function의 별도 객체다.

## 13. 다음 Step
[Step 10-4. 반환값과 `void`](10-4-return-void.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.2.2, 6.9.1
- [cppreference: Function call](https://en.cppreference.com/w/c/language/operator_other.html#Function_call)
