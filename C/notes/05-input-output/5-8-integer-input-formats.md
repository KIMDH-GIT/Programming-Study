# 5-8. 정수 입력 서식 지정자

정수 입력에서도 변환 지정과 저장 대상형이 정확히 맞아야 한다. 출력과 달리 `scanf` 인자는 해당 객체를 가리키는 포인터다.

## 1. 학습 목표

- `%d`, `%u`, `%x`의 정수 입력 의미를 구별한다.
- `int *`와 `unsigned int *` 저장 대상을 맞춘다.
- 입력 진법과 저장값을 구별한다.

## 2. 선수 지식

Step 5-2의 정수 출력과 Step 5-7의 주소·반환값을 사용한다.

## 3. 핵심 개념

| 지정 | 읽는 표기 | 저장 인자 |
|---|---|---|
| `%d` | signed 10진 | `int *` |
| `%u` | unsigned 10진 | `unsigned int *` |
| `%x` | unsigned 16진 | `unsigned int *` |

`%x`는 `2a`나 구현 규칙에 맞는 `0x2a`를 읽어 값 42로 저장할 수 있다. 반환값이 3이어야 세 객체 모두 새 입력값을 받았다고 말할 수 있다.

## 4. 문법

```c
matched = scanf("%d %u %x", &a, &b, &c);
```

## 5. 최소 코드 예제

```c
#include <stdio.h>

int main(void)
{
    int decimal = 0;
    unsigned int unsigned_decimal = 0U;
    unsigned int hexadecimal = 0U;
    int matched;

    matched = scanf("%d %u %x",
                    &decimal, &unsigned_decimal, &hexadecimal);
    printf("matched=%d values=%d,%u,0x%x\n",
           matched, decimal, unsigned_decimal, hexadecimal);
    return 0;
}
```

## 6. 코드 해석

1. 세 저장 객체를 안전한 값으로 초기화한다.
2. 각 주소형이 변환 지정과 일치한다.
3. 공백은 입력에서 연속 공백 문자를 건너뛴다.
4. 출력은 저장된 세 수학적 값을 확인한다.

## 7. 내부 동작

`scanf`는 입력 prefix를 각 진법으로 해석하고 표현 가능한 경우 객체에 변환 결과를 저장한다. 입력값이 대상형 범위를 벗어날 때의 동작에 의존하지 말아야 하며, 견고한 숫자 파싱은 이후 단계에서 더 적합한 함수를 배운다.

## 8. 자주 하는 실수

- `%u`에 `int *`를 넘긴다.
- `%x`에 signed 객체 주소를 넘긴다.
- 16진 입력 문자열과 메모리 표현을 같은 것으로 본다.
- 반환값 확인 없이 세 값이 모두 대입됐다고 가정한다.

## 9. 필수 실습

signed 10진, unsigned 10진, unsigned 16진 값을 한 줄에서 입력한다. [실습 README](../../exercises/05-input-output/5-8/README.md)를 따른다.

## 10. 추가 실습

- ★ **기초:** `-12 12 2a`를 입력한다.
- ★★ **응용:** 두 번째 변환에서 실패시킨다.
- ★★★ **도전:** 범위를 벗어난 입력을 안전하다고 가정하면 안 되는 이유를 조사한다.

## 11. 확인 문제

1. `%u`의 저장 인자형은 무엇인가?
2. `%x`는 어떤 진법을 읽는가?
3. 세 변환이 성공하면 반환값은 얼마인가?
4. 반환값이 1이면 어느 객체까지 대입됐다고 말할 수 있는가?

## 12. 핵심 정리

정수 입력 지정은 읽는 진법과 저장 대상형을 함께 정한다. 주소형을 정확히 맞추고 반환값으로 성공한 대입 수를 확인한다.

## 13. 다음 Step

[Step 5-9. `scanf`의 `%f`, `%lf`, `%Lf`](5-9-floating-input-formats.md)

## 14. 참고 자료

- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 7.21.6.2.
- [cppreference: `scanf`](https://en.cppreference.com/w/c/io/fscanf.html)
- [GCC Format Warnings](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html#index-Wformat)
