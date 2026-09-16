# 5-13. 계산 실습용 산술 연산자와 나눗셈 예고

뒤의 계산 실습에 필요한 `+`, `-`, `*`, `/`, `%`를 먼저 제한적으로 사용한다. 연산자 전체 규칙과 변환은 Part 6·7에서 자세히 배운다.

## 1. 학습 목표

- 이항 산술 연산자의 피연산자와 결과를 읽는다.
- 정수 나눗셈과 실수 나눗셈을 구별한다.
- 0으로 나누기를 피한다.

## 2. 선수 지식

정수·부동소수점 변수, 식, 출력 서식을 사용한다.

## 3. 핵심 개념

`a + b`, `a - b`, `a * b`, `a / b`는 각각 합·차·곱·몫을 만든다. 정수끼리의 `/`는 0 방향으로 버린 정수 몫이며 `%`는 나머지다. `double` 피연산자가 있는 나눗셈은 실수 결과를 만든다.

0으로 정수 나눗셈을 하면 UB다. signed 최솟값을 -1로 나누는 경계도 Part 7에서 배운다.

## 4. 문법

```c
int quotient = left / right;
int remainder = left % right;
double ratio = real_left / real_right;
```

## 5. 최소 코드 예제

```c
#include <stdio.h>

int main(void)
{
    int left = 17;
    int right = 5;
    double real_left = 17.0;
    double real_right = 5.0;

    printf("%d %d %d %d %d\n",
           left + right, left - right, left * right,
           left / right, left % right);
    printf("%.2f\n", real_left / real_right);
    return 0;
}
```

## 6. 코드 해석

정수 몫은 3, 나머지는 2다. 실수 나눗셈은 약 3.40으로 표시된다. 같은 `/`라도 피연산자형이 결과 의미를 바꾼다.

## 7. 내부 동작

C의 추상 기계가 형에 따른 산술 규칙을 정한다. CPU 명령 선택은 컴파일러의 몫이며 MIPS나 RISC-V 설명은 이번 개념에 필요하지 않다.

## 8. 자주 하는 실수

- `17 / 5`가 3.4라고 생각한다.
- 나머지 `%`를 백분율 기호로 해석한다.
- 0으로 나눠 오류값이 반환된다고 생각한다.
- 출력 반올림과 산술 결과를 혼동한다.

## 9. 필수 실습

고정된 두 정수와 두 실수로 산술 결과를 출력한다. [실습 README](../../exercises/05-input-output/5-13/README.md)를 따른다.

## 10. 추가 실습

- ★ **기초:** 20과 6으로 반복한다.
- ★★ **응용:** 음수 정수 나눗셈을 관찰한다.
- ★★★ **도전:** 0 나눗셈을 실행하지 않고 UB 이유를 쓴다.

## 11. 확인 문제

1. `17 / 5`의 정수 결과는 무엇인가?
2. `17 % 5`는 무엇인가?
3. `17.0 / 5.0`은 왜 다른가?
4. 정수 0 나눗셈의 동작 분류는 무엇인가?

## 12. 핵심 정리

산술 결과는 피연산자형에 달린다. 정수 `/`는 정수 몫, `%`는 나머지이며 0으로 나누면 안 된다.

## 13. 다음 Step

[Step 5-14. 두 수의 사칙연산](5-14-two-number-arithmetic.md)

## 14. 참고 자료

- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.5~6.5.6.
- [cppreference: arithmetic operators](https://en.cppreference.com/w/c/language/operator_arithmetic.html)
- [GCC Integer Implementation](https://gcc.gnu.org/onlinedocs/gcc/Integers-implementation.html)
