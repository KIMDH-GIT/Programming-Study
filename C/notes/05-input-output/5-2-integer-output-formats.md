# 5-2. `%d`, `%u`, `%x`

같은 비트 패턴이나 수학적 값도 signed 여부와 출력 진법에 따라 다른 문자로 표시된다. `%d`, `%u`, `%x`는 모두 `int` 계열 인자와 정확히 대응해야 한다.

## 1. 학습 목표

- `%d`, `%u`, `%x`의 요구 인자형과 출력 진법을 구별한다.
- `0x` 접두사가 자동으로 붙지 않음을 설명한다.
- 형식 불일치와 값 표기 차이를 구별한다.

## 2. 선수 지식

Part 2의 `int`, `unsigned int`, Part 3의 10진·16진 표기와 Step 5-1의 인자 대응을 사용한다.

## 3. 핵심 개념

| 지정 | 요구 인자 | 표시 |
|---|---|---|
| `%d` | `int` | signed 10진 |
| `%u` | `unsigned int` | unsigned 10진 |
| `%x` | `unsigned int` | 소문자 16진 |

`%x`는 숫자만 출력한다. `0x`가 필요하면 `"0x%x"`처럼 일반 문자를 직접 쓴다. 필드 폭과 0 채움은 `%08x`처럼 지정할 수 있다.

## 4. 문법

```c
printf("%d %u 0x%x\n", signed_value, unsigned_value, unsigned_value);
```

## 5. 최소 코드 예제

```c
#include <stdio.h>

int main(void)
{
    int signed_value = -42;
    unsigned int unsigned_value = 42U;

    printf("signed=%d\n", signed_value);
    printf("unsigned=%u\n", unsigned_value);
    printf("hex=0x%08x\n", unsigned_value);
    return 0;
}
```

## 6. 코드 해석

1. `-42`는 `%d`와 대응한다.
2. `42U`는 `unsigned int`이므로 `%u`, `%x`와 대응한다.
3. `%08x`는 최소 8자리를 0으로 채운다.
4. `0x`는 서식 문자열의 일반 문자다.

## 7. 내부 동작

`printf`는 정수값을 요청된 진법의 문자열로 변환한다. 출력된 문자 순서는 객체의 메모리 byte 순서를 보여 주지 않는다. 기본 정수형의 구체적 폭은 구현에 따라 달라진다.

## 8. 자주 하는 실수

- signed `int`를 `%u`나 `%x`에 그대로 넘긴다.
- `%x`가 `0x`를 자동 출력한다고 생각한다.
- `%08x`의 8을 정수형 bit 폭으로 이해한다.
- 16진 출력으로 endianness를 판단한다.

## 9. 필수 실습

같은 `unsigned int` 값을 10진과 16진으로 출력한다. [실습 README](../../exercises/05-input-output/5-2/README.md)를 사용한다.

## 10. 추가 실습

- ★ **기초:** 255U를 `%u`, `%x`로 출력한다.
- ★★ **응용:** `%8x`와 `%08x`를 비교한다.
- ★★★ **도전:** 객체 표현과 출력 표기가 다른 이유를 쓴다.

## 11. 확인 문제

1. `%d`가 요구하는 형은 무엇인가?
2. `%u`와 `%x`의 값 차이는 무엇인가?
3. `%x`가 접두사 `0x`를 자동 출력하는가?
4. `%08x`의 8은 무엇을 뜻하는가?

## 12. 핵심 정리

`%d`는 `int`, `%u`와 `%x`는 `unsigned int`에 대응한다. `%u`와 `%x`는 같은 값을 서로 다른 진법으로 표시한다.

## 13. 다음 Step

[Step 5-3. `%f`, `%Lf`와 출력 정밀도](5-3-floating-output-precision.md)

## 14. 참고 자료

- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 7.21.6.1.
- [cppreference: `printf`](https://en.cppreference.com/w/c/io/fprintf.html)
- [GCC Format Warnings](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html#index-Wformat)
