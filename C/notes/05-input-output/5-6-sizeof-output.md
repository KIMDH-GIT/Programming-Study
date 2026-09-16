# 5-6. `%zu`와 `sizeof` 출력

`sizeof`의 결과형은 `size_t`다. 따라서 결과를 출력할 때는 C99 이후 표준인 `%zu`를 사용하고, 결과 단위가 bit가 아니라 C byte임을 유지한다.

## 1. 학습 목표

- `sizeof` 결과형과 단위를 설명한다.
- `size_t` 값에 `%zu`를 사용한다.
- 형과 식에 적용하는 두 문법을 구별한다.

## 2. 선수 지식

Part 2의 `sizeof`, C byte, `CHAR_BIT`와 Step 5-1의 형식 대응을 사용한다.

## 3. 핵심 개념

`sizeof(int)`와 `sizeof value`는 모두 `size_t` 값을 만든다. 결과는 객체 표현이 차지하는 C byte 수다. `sizeof(char)`는 항상 1이지만 한 C byte의 bit 수는 `CHAR_BIT`다.

대부분의 피연산자는 실행 중 평가되지 않는다. 가변 길이 배열은 예외지만 아직 다루지 않는다.

## 4. 문법

```c
printf("%zu\n", sizeof(int));
printf("%zu\n", sizeof value);
```

## 5. 최소 코드 예제

```c
#include <limits.h>
#include <stdio.h>

int main(void)
{
    double value = 1.0;

    printf("int bytes=%zu\n", sizeof(int));
    printf("value bytes=%zu\n", sizeof value);
    printf("CHAR_BIT=%d\n", CHAR_BIT);
    return 0;
}
```

## 6. 코드 해석

1. `sizeof(int)`는 형 이름 문법이므로 괄호가 필요하다.
2. `sizeof value`는 식 문법이라 괄호가 필수는 아니다.
3. 두 결과의 형은 `size_t`이고 `%zu`와 맞는다.
4. `CHAR_BIT`가 byte당 bit 수를 제공한다.

## 7. 내부 동작

`sizeof` 값은 대상 구현의 자료형 배치 규칙에서 정해진다. 기본형 크기는 실행 중 측정할 필요 없이 번역 시 알려진 상수 표현이다. CPU 이름만으로 C 자료형 크기를 단정하지 않는다.

## 8. 자주 하는 실수

- `sizeof` 결과를 `%d`로 출력한다.
- 결과 숫자를 bit 수로 읽는다.
- `sizeof(char) == 1`을 8 bit 보장으로 해석한다.
- 형 이름에 괄호를 생략한다.

## 9. 필수 실습

기본형 몇 개의 크기와 `CHAR_BIT`를 출력한다. [실습 README](../../exercises/05-input-output/5-6/README.md)를 따른다.

## 10. 추가 실습

- ★ **기초:** `char`, `short`, `long` 크기를 출력한다.
- ★★ **응용:** C byte 수와 저장 bit 수를 따로 계산한다.
- ★★★ **도전:** 다른 ABI의 결과를 구현 관찰로 비교한다.

## 11. 확인 문제

1. `sizeof` 결과형은 무엇인가?
2. `%zu`는 어떤 값에 사용하는가?
3. `sizeof` 결과 단위는 무엇인가?
4. `sizeof(char) == 1`은 무엇을 보장하는가?

## 12. 핵심 정리

`sizeof`는 `size_t` 형의 C byte 수를 만들며 `%zu`로 출력한다. bit 수가 필요하면 `CHAR_BIT`와 구별해 연결한다.

## 13. 다음 Step

[Step 5-7. `scanf` 구조와 변수 주소](5-7-scanf-and-addresses.md)

## 14. 참고 자료

- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.3.4, 7.19.
- [cppreference: `sizeof`](https://en.cppreference.com/w/c/language/sizeof.html)
- [cppreference: `printf`](https://en.cppreference.com/w/c/io/fprintf.html)
