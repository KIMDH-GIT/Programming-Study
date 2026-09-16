# 5-16. 섭씨·화씨 변환

온도 변환은 상수의 형이 계산 결과에 미치는 영향을 보여 준다. 실수 상수를 사용해 비율 `9.0 / 5.0`을 유지한다.

## 1. 학습 목표

- 섭씨에서 화씨로 변환하는 식을 작성한다.
- 실수 상수로 나눗셈 의미를 명확히 한다.
- 입력·계산·출력을 구별한다.

## 2. 선수 지식

`double`, 산술 연산, `%f` 출력 정밀도를 사용한다.

## 3. 핵심 개념

```text
화씨 = 섭씨 × 9 / 5 + 32
```

`9.0 / 5.0`은 실수 비율을 분명히 표현한다. `9 / 5`를 먼저 계산하면 정수 결과 1이 되어 의도한 비율을 잃는다.

## 4. 문법

```c
double fahrenheit = celsius * 9.0 / 5.0 + 32.0;
```

## 5. 최소 코드 예제

```c
#include <stdio.h>

int main(void)
{
    double celsius = 25.0;
    double fahrenheit = celsius * 9.0 / 5.0 + 32.0;

    printf("%.2f C = %.2f F\n", celsius, fahrenheit);
    return 0;
}
```

## 6. 코드 해석

섭씨값과 실수 상수로 화씨값을 계산한다. 25도는 77도로 표시된다. 두 출력 인자는 `double`이다.

## 7. 내부 동작

부동소수점 근사와 출력 반올림이 적용될 수 있다. 형 변환의 일반 순서는 Part 6에서 자세히 다룬다.

## 8. 자주 하는 실수

- `9 / 5`를 별도 정수식으로 계산한다.
- 32를 곱셈 전에 더한다.
- 입력 단위와 출력 단위를 바꿔 쓴다.
- 출력 정밀도를 저장 정밀도로 본다.

## 9. 필수 실습

고정 섭씨값을 화씨로 변환한다. [실습 README](../../exercises/05-input-output/5-16/README.md)를 따른다.

## 10. 추가 실습

- ★ **기초:** 0°C를 변환한다.
- ★★ **응용:** -40°C를 확인한다.
- ★★★ **도전:** 역변환 식을 설계한다.

## 11. 확인 문제

1. `9.0 / 5.0`을 쓰는 이유는 무엇인가?
2. 0°C는 몇 °F인가?
3. 덧셈 32의 위치는 어디인가?
4. 출력 정밀도가 계산식을 바꾸는가?

## 12. 핵심 정리

온도 변환에서는 올바른 식 순서와 실수 비율을 유지해야 한다. 표시 정밀도는 계산형과 별개다.

## 13. 다음 Step

[Step 5-17. 초를 시·분·초로 변환](5-17-seconds-conversion.md)

## 14. 참고 자료

- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.5~6.5.6.
- [cppreference: arithmetic operators](https://en.cppreference.com/w/c/language/operator_arithmetic.html)
- [cppreference: `printf`](https://en.cppreference.com/w/c/io/fprintf.html)
