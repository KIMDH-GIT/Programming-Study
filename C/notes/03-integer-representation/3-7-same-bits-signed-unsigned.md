# Step 3-7. 같은 비트 패턴의 signed·unsigned 해석

C17을 기준으로 종이에 그린 같은 0·1 배열도 선택한 정수 표현 규칙에 따라 다른 값이 될 수 있음을 배운다. 값 변환을 객체의 bit 재해석으로 오해하지 않는다.

## 1. 학습 목표

- 비트 패턴과 그 패턴에 부여하는 값을 구별한다.
- 가상의 8-bit 모형을 unsigned와 세 signed 표현으로 읽는다.
- 대응 signed·unsigned 형의 공통 비음수 값에 대한 C17 보장을 설명한다.
- 정수 cast와 초기화가 값 변환임을 설명한다.
- unsigned 목적지와 범위 밖 signed 목적지 변환을 구별한다.

## 2. 선수 지식

[3-6. 2의 보수 모델과 C17 signed 표현](3-6-twos-complement-c17-signed-representations.md), [2-4. signed와 unsigned](../02-variables-and-types/2-4-signed-unsigned.md), `<limits.h>`의 `UINT_MAX`를 사용한다.

이번 표는 객체의 byte를 읽은 결과가 아니다. 배열, 포인터, union, 비트 연산자, shift, endianness는 이후 Part에서 다룬다.

## 3. 핵심 개념

패턴만으로 signed 값을 정하려면 폭, padding 여부, 표현 방식, 예외 패턴의 의미가 더 필요하다.

**[가상의 padding 없는 8-bit 표현 예 - 실제 C 객체 관찰 아님]**

| 패턴 | unsigned | 부호와 크기 | 1의 보수 | 2의 보수 |
|---|---:|---:|---:|---:|
| `00000101` | 5 | 5 | 5 | 5 |
| `10000101` | 133 | -5 | -122 | -123 |
| `11111111` | 255 | -127 | 음의 0 후보 | -1 |

**[C17 표준]** 대응하는 signed·unsigned 형이 공통으로 표현하는 비음수 값은 값과 표현이 같다. 그러나 이것은 모든 음수와 unsigned 값이 같은 bit를 공유한다거나 변환이 byte 복사라는 뜻이 아니다.

### 값 변환은 재해석이 아니다

목적지 형이 원래 값을 표현할 수 있으면 값은 유지된다. 목적지가 unsigned이고 원래 값이 범위 밖이면 최댓값보다 1 큰 수를 반복해서 더하거나 빼는 것과 같은 결과를 얻는다. 따라서 `(unsigned int)-1 == UINT_MAX`다.

목적지가 signed이고 원래 값을 표현할 수 없으면 결과는 구현 정의 값이거나 구현 정의 신호가 발생한다. 이것은 signed 산술 overflow의 UB와도 다른 규칙이다.

## 4. 문법

```c
int signed_common = 42;
unsigned int unsigned_common = signed_common;
unsigned int converted = (unsigned int)-1;
```

cast는 원래 객체의 저장 bit를 유지하라는 명령이 아니다. `%d`와 `%u`도 bit 재해석 선택기가 아니므로 실제 인수형과 맞춘다.

## 5. 최소 코드 예제

`integer_conversion.c`에 저장한다.

```c
#include <limits.h>
#include <stdio.h>

int main(void)
{
    int signed_common = 42;
    unsigned int unsigned_common = signed_common;
    int signed_again = unsigned_common;
    unsigned int from_minus_one = (unsigned int)-1;

    printf("common=%d,%u,%d\n",
           signed_common, unsigned_common, signed_again);
    printf("converted_minus_one=%u\n", from_minus_one);
    printf("UINT_MAX=%u\n", UINT_MAX);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic integer_conversion.c -o integer_conversion && ./integer_conversion
```

## 6. 코드 해석

42는 양쪽 형의 공통 범위라 왕복 뒤에도 유지된다. -1의 unsigned 변환 결과는 signed 표현 방식과 무관하게 `UINT_MAX`다. 예제는 범위 밖 unsigned-to-signed 변환을 실행하지 않는다.

## 7. 내부 동작

- **[C 추상 기계]** 식의 값을 목적지 정수형의 값으로 변환한다.
- **[객체 표현]** 값 저장에는 목적지 형의 표현이 쓰이지만 원본 byte 보존은 약속되지 않는다.
- **[컴파일러]** 상수로 계산하거나 레지스터를 사용할 수 있어 별도 메모리 복사가 없을 수 있다.
- **[구현]** 범위 밖 signed 목적지 결과와 실제 signed 표현은 구현 문서로 확인한다.
- **[출력]** 서식은 인수형을 바꾸지 않는다.

## 8. 자주 하는 실수

- `11111111`은 언제나 -1이라고 한다.
- cast가 같은 bit를 다른 형으로 읽는다고 한다.
- unsigned 변환을 절댓값 변환이라고 한다.
- 범위 밖 signed 변환을 항상 -1 또는 항상 UB라고 한다.
- `%d`와 `%u`만 바꾸면 안전하게 재해석된다고 한다.
- 가상 8-bit 표를 실제 `int`의 메모리 배치라고 한다.

## 9. 필수 실습

- **목적:** 정의된 값 변환과 가상 패턴 해석을 분리한다.
- **작성할 파일:** `exercises/03-integer-representation/3-7/integer_conversion.c`.
- **해야 할 일:** [README](../../exercises/03-integer-representation/3-7/README.md)에 따라 42의 왕복과 -1의 unsigned 변환을 출력하고 가상 8-bit 표를 종이에 작성한다.
- **예상 관찰:** 42는 유지되고 변환한 -1은 `UINT_MAX`와 같다.
- **확인 포인트:** 변환을 bit 복사라고 하지 않았는가?

## 10. 추가 실습

- ★ 0, 1, 127의 공통 범위 변환
- ★★ `10000001`, `11111110`의 네 모형 비교
- ★★★ 범위 밖 signed 변환의 구현 문서 조사

## 11. 확인 문제

1. 같은 패턴이 다른 값을 가질 수 있는 이유는?
2. 가상 8-bit 2의 보수에서 `11111110`은 얼마인가?
3. C17은 모든 signed 표현을 2의 보수로 고정하는가?
4. `(unsigned int)-1`의 값과 근거는?
5. `UINT_MAX`가 `int`에 안 들어갈 때 변환의 분류는?
6. `%u`가 `int` 인수를 자동 변환하는가?

<details><summary>정답과 해설</summary>

1. 폭과 표현 규칙이 다르기 때문이다. 2. -2다. 3. 아니다. 4. `UINT_MAX`, unsigned 목적지 변환 규칙 때문이다. 5. 구현 정의 결과 또는 구현 정의 신호다. 6. 아니다.

</details>

## 12. 핵심 정리

패턴은 폭과 해석 규칙을 함께 지정해야 값을 갖는다. C 정수 변환은 값 변환이며 bit 재해석이 아니다. unsigned 목적지 변환은 modulo 규칙을 따르고 범위 밖 signed 목적지 변환은 구현 정의다.

## 13. 다음 Step

[Step 3-8. unsigned 순환과 signed overflow](3-8-unsigned-wrap-signed-overflow.md)

## 14. 참고 자료

- [WG14 N2176 C17 투표 초안](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.2.5, 6.2.6.2, 6.3.1.3.
- [WG14 N1570](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf): 같은 규칙의 C11 공개 초안이며 C17 최종 원문이 아니다.
- [GCC Integers](https://gcc.gnu.org/onlinedocs/gcc/Integers-implementation.html)
