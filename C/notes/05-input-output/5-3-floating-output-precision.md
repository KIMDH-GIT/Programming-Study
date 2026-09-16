# 5-3. `%f`, `%Lf`와 출력 정밀도

부동소수점 출력에서는 인자형과 표시할 소수 자릿수를 따로 생각한다. `float`는 가변 인수 호출에서 `double`로 변환되지만 `long double`은 `%Lf`가 필요하다.

## 1. 학습 목표

- `double` 출력의 `%f`와 `long double` 출력의 `%Lf`를 구별한다.
- 정밀도 표기 `%.nf`의 의미를 설명한다.
- 출력 자릿수와 저장 정밀도를 구별한다.

## 2. 선수 지식

Part 2의 세 부동소수점형과 Step 5-1의 인자 대응을 사용한다. 형 변환의 일반 규칙은 Part 6에서 배운다.

## 3. 핵심 개념

`printf` 호출에서 `float` 인자는 기본 인수 승격으로 `double`이 되므로 `%f`를 사용한다. `double`도 `%f`, `long double`은 `%Lf`다. `%.2f`의 2는 소수점 뒤에 표시할 자리 수이며 객체의 저장 정밀도를 바꾸지 않는다.

표시 과정에서 다음 자리의 값에 따라 반올림된 문자 결과가 나타날 수 있다. 십진 소수를 이진 부동소수점으로 정확히 표현하지 못하는 경우도 있다.

## 4. 문법

```c
printf("%.2f %.3Lf\n", double_value, long_double_value);
```

## 5. 최소 코드 예제

```c
#include <stdio.h>

int main(void)
{
    float small = 1.25F;
    double normal = 3.141592;
    long double wide = 2.718281828L;

    printf("float=%.2f\n", small);
    printf("double=%.4f\n", normal);
    printf("long double=%.6Lf\n", wide);
    return 0;
}
```

## 6. 코드 해석

1. `small`은 호출에서 `double`로 승격되어 `%f`와 맞는다.
2. `normal`은 소수점 뒤 네 자리로 표시된다.
3. `wide`는 `long double`이므로 `%Lf`가 필요하다.
4. 출력 정밀도는 변수의 형이나 저장값을 변경하지 않는다.

## 7. 내부 동작

**[C 언어]** 가변 인수에 기본 인수 승격이 적용된다. **[라이브러리]** `printf`가 이진 부동소수점 값을 십진 문자로 변환한다. `long double`의 실제 표현과 정밀도는 구현에 따라 다를 수 있다.

## 8. 자주 하는 실수

- `printf`에서 `float`에 별도의 `%lf`가 필요하다고 생각한다.
- `long double`을 `%f`로 출력한다.
- `%.2f`가 값을 소수 둘째 자리로 저장한다고 생각한다.
- 화면의 반올림 결과를 원래 값 변경으로 해석한다.

## 9. 필수 실습

세 부동소수점형을 서로 다른 정밀도로 출력한다. [실습 README](../../exercises/05-input-output/5-3/README.md)를 따른다.

## 10. 추가 실습

- ★ **기초:** 같은 `double`을 `%.1f`, `%.6f`로 출력한다.
- ★★ **응용:** 0.1을 여러 자릿수로 관찰한다.
- ★★★ **도전:** 출력 정밀도와 `<float.h>`의 정밀도를 비교한다.

## 11. 확인 문제

1. `printf`에서 `float`가 `%f`와 맞는 이유는 무엇인가?
2. `long double`의 서식은 무엇인가?
3. `%.3f`의 3은 무엇을 뜻하는가?
4. 출력 정밀도가 변수의 저장값을 바꾸는가?

## 12. 핵심 정리

`printf`에서 `float`와 `double`은 `%f`, `long double`은 `%Lf`다. 정밀도는 표시할 소수 자릿수를 제어할 뿐 저장형을 바꾸지 않는다.

## 13. 다음 Step

[Step 5-4. `%c`, `%s`](5-4-character-string-output.md)

## 14. 참고 자료

- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.2.2, 7.21.6.1.
- [cppreference: `printf`](https://en.cppreference.com/w/c/io/fprintf.html)
- [cppreference: default argument promotions](https://en.cppreference.com/w/c/language/conversion.html)
