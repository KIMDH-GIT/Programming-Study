# 5-9. `scanf`의 `%f`, `%lf`, `%Lf`

`scanf`에서는 `float`, `double`, `long double`의 저장 크기가 다르므로 세 서식이 각각 다른 포인터형을 요구한다. 출력 규칙과 혼동하면 안 된다.

## 1. 학습 목표

- `%f`, `%lf`, `%Lf`의 저장 대상형을 구별한다.
- `printf`와 `scanf`의 `%f` 규칙 차이를 설명한다.
- 반환값으로 세 대입의 성공 여부를 판단한다.

## 2. 선수 지식

Step 5-3의 부동소수점 출력과 Step 5-7의 주소·반환값을 사용한다.

## 3. 핵심 개념

| 입력 지정 | 저장 인자 |
|---|---|
| `%f` | `float *` |
| `%lf` | `double *` |
| `%Lf` | `long double *` |

`printf`에서는 `float`가 `double`로 승격되어 `%f`를 썼지만, `scanf`는 객체에 직접 저장하므로 `%f`와 `%lf`를 구별한다.

## 4. 문법

```c
matched = scanf("%f %lf %Lf", &a, &b, &c);
```

## 5. 최소 코드 예제

```c
#include <stdio.h>

int main(void)
{
    float a = 0.0F;
    double b = 0.0;
    long double c = 0.0L;
    int matched;

    matched = scanf("%f %lf %Lf", &a, &b, &c);
    printf("matched=%d values=%.2f,%.2f,%.2Lf\n",
           matched, a, b, c);
    return 0;
}
```

## 6. 코드 해석

1. 세 객체는 서로 다른 형이다.
2. 입력 지정은 각 객체 주소형과 일치한다.
3. 출력에서는 `a`가 `double`로 승격되어 `%f`와 맞는다.
4. `c`는 입력과 출력 모두 대문자 `L`이 필요하다.

## 7. 내부 동작

입력 함수는 문자 sequence를 대상 부동소수점형으로 변환해 저장한다. 표현 가능 범위와 정밀도는 구현의 부동소수점 형에 의존하며 `<float.h>`로 확인할 수 있다.

## 8. 자주 하는 실수

- `double *`에 입력 `%f`를 사용한다.
- `float *`에 `%lf`를 사용한다.
- `printf`와 `scanf` 규칙이 같다고 외운다.
- 반환값 없이 세 값의 성공을 가정한다.

## 9. 필수 실습

세 형의 값을 한 줄에서 입력하고 각기 알맞게 출력한다. [실습 README](../../exercises/05-input-output/5-9/README.md)를 따른다.

## 10. 추가 실습

- ★ **기초:** 같은 십진 문자열을 세 형에 입력한다.
- ★★ **응용:** 긴 소수를 입력해 표시를 비교한다.
- ★★★ **도전:** `<float.h>` 한계와 관찰값을 연결한다.

## 11. 확인 문제

1. `scanf`의 `%f`가 요구하는 포인터형은 무엇인가?
2. `%lf`는 어떤 객체 주소와 맞는가?
3. 출력에서 `float`가 `%f`와 맞는 이유는 무엇인가?
4. 세 대입 성공 시 반환값은 얼마인가?

## 12. 핵심 정리

입력에서는 `%f`→`float *`, `%lf`→`double *`, `%Lf`→`long double *`다. 출력의 기본 인수 승격 규칙과 분리해 기억한다.

## 13. 다음 Step

[Step 5-10. 문자·문자열 입력과 buffer 한계](5-10-character-string-input.md)

## 14. 참고 자료

- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 7.21.6.1~2.
- [cppreference: `scanf`](https://en.cppreference.com/w/c/io/fscanf.html)
- [cppreference: `printf`](https://en.cppreference.com/w/c/io/fprintf.html)
