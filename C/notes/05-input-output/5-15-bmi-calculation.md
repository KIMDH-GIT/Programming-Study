# 5-15. BMI 계산

BMI 계산은 실수 입력값, 산술식, 출력 정밀도를 연결하는 작은 예제다. 단위와 식의 피연산자형을 명확히 한다.

## 1. 학습 목표

- kg과 m 단위를 사용해 BMI 식을 작성한다.
- 실수 나눗셈을 유지한다.
- 결과를 적절한 소수 자릿수로 출력한다.

## 2. 선수 지식

`double`, `%f`, 곱셈과 나눗셈을 사용한다. BMI 판정 구간은 조건문을 배우지 않았으므로 다루지 않는다.

## 3. 핵심 개념

```text
BMI = 몸무게(kg) / (키(m) × 키(m))
```

키는 0보다 커야 한다. cm 값을 그대로 넣으면 단위가 틀린다. 이번 예제는 유효한 고정 데이터를 사용해 계산 구조에 집중한다.

## 4. 문법

```c
double bmi = weight_kg / (height_m * height_m);
```

## 5. 최소 코드 예제

```c
#include <stdio.h>

int main(void)
{
    double weight_kg = 68.0;
    double height_m = 1.75;
    double bmi = weight_kg / (height_m * height_m);

    printf("BMI=%.2f\n", bmi);
    return 0;
}
```

## 6. 코드 해석

키를 제곱한 뒤 몸무게를 나눈다. 모든 피연산자가 `double`이므로 실수 나눗셈이다. 출력은 소수 둘째 자리까지 표시한다.

## 7. 내부 동작

이진 부동소수점 근사 때문에 수학적 실수와 저장값이 정확히 같지 않을 수 있다. `%.2f`는 표시를 반올림할 뿐 BMI 객체를 바꾸지 않는다.

## 8. 자주 하는 실수

- cm를 m로 바꾸지 않는다.
- 키를 한 번만 곱한다.
- 정수형만 사용해 중간 결과를 잃는다.
- 출력값만으로 의료 판단을 추가한다.

## 9. 필수 실습

유효한 kg·m 고정값으로 BMI를 계산한다. [실습 README](../../exercises/05-input-output/5-15/README.md)를 따른다.

## 10. 추가 실습

- ★ **기초:** 다른 유효한 값을 사용한다.
- ★★ **응용:** 출력 정밀도를 비교한다.
- ★★★ **도전:** 입력 버전에 필요한 유효성 조건을 적는다.

## 11. 확인 문제

1. 키 단위는 무엇이어야 하는가?
2. 키가 0이면 어떤 문제가 생기는가?
3. 식이 실수 나눗셈이 되는 이유는 무엇인가?
4. `%.2f`가 저장값을 바꾸는가?

## 12. 핵심 정리

BMI 계산은 단위, 유효한 제수, 실수 피연산자, 출력 정밀도를 함께 확인해야 한다.

## 13. 다음 Step

[Step 5-16. 섭씨·화씨 변환](5-16-temperature-conversion.md)

## 14. 참고 자료

- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.5.
- [cppreference: floating arithmetic](https://en.cppreference.com/w/c/language/operator_arithmetic.html)
- [cppreference: `printf`](https://en.cppreference.com/w/c/io/fprintf.html)
