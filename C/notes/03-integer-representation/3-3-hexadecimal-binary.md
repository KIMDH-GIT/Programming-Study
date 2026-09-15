# Step 3-3. 16진수와 2진수

기준은 C17 호스트 환경이다. 16진수 한 자리를 이진수 네 자리로 바꾸며, 숫자 표기와 객체의 byte 배치를 구별한다.

## 1. 학습 목표
- `0`~`9`, `A`~`F`의 값을 읽는다.
- 16진 한 자리와 이진 네 자리의 관계를 사용한다.
- 16진수·이진수·십진수가 같은 값을 나타낼 수 있음을 설명한다.
- 숫자 표기와 객체 표현을 구별한다.

## 2. 선수 지식
[3-2](3-2-decimal-binary.md)의 이진 자릿값과 `%u`를 사용한다. `%X`는 필요한 만큼만 먼저 사용한다.

## 3. 핵심 개념
16은 `2⁴`이므로 16진 한 자리는 이진 네 자리와 정확히 대응한다.

```text
0=0000  5=0101  A=1010  F=1111
0xA5 = 1010 0101₂ = 10×16 + 5 = 165₁₀
```

앞의 `0x`는 C와 여러 표기 관례에서 16진수를 알리는 접두사다. 두 hex digit이 여덟 이진 자리를 나타내지만, 한 C byte가 항상 8 bit라는 뜻은 아니다.

**[구현과 후속 학습]** `0x12345678`의 왼쪽부터 읽는 숫자 표기와 다중 byte 객체가 메모리에 놓이는 순서는 별개다. endianness는 Part 20에서 다룬다.

## 4. 문법
`%X`는 `unsigned int` 값을 대문자 16진수로, `%x`는 소문자로 출력한다. `0xA5U`는 unsigned 후보를 사용하는 16진 정수 상수다.

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    unsigned int value = 0xA5U;
    printf("decimal=%u\n", value);
    printf("hex=0x%X\n", value);
    printf("binary notation=1010 0101\n");
    return 0;
}
```
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic hex_binary.c -o hex_binary && ./hex_binary
```

## 6. 코드 해석
`value`의 값은 165다. `%u`와 `%X`는 같은 값을 다른 진법의 문자로 출력한다. 이진 문자열은 사람이 확인할 설명이며 객체 표현을 검사하지 않는다.

## 7. 내부 동작
- **[C 표준]** 16진 정수 상수와 `%X` 출력 변환을 정의한다.
- **[컴파일러]** 상수의 값과 형을 정해 대상 코드로 번역한다.
- **[메모리]** 숫자 표기만으로 byte 순서를 알 수 없다.
- **[CPU]** 4-bit 묶음은 읽기 편한 표기이며 특정 명령을 요구하지 않는다.

## 8. 자주 하는 실수
- `A`를 십진 10이 아닌 문자 이름으로만 본다.
- `0xA5`를 `10×10+5`로 계산한다.
- hex 두 자리를 모든 C 구현의 한 byte라고 한다.
- 숫자를 적는 순서를 endianness라고 한다.
- `%X`에 signed `int`를 그대로 전달한다.

## 9. 필수 실습
- **목적:** 같은 값을 10진·16진·이진으로 연결한다.
- **작성할 파일:** `hex_binary.c`.
- **해야 할 일:** [README](../../exercises/03-integer-representation/3-3/README.md)에 따라 `0xA5U`를 `%u`, `%X`로 출력하고 네 bit씩 변환표를 작성한다.
- **예상 관찰:** 165와 A5가 같은 값이다.

## 10. 추가 실습
- ★ `0x2D`를 이진수와 십진수로 바꾼다.
- ★★ `1111 0000₂`를 16진수로 바꾼다.
- ★★★ 앞의 0이 값에 영향을 주지 않는 이유를 설명한다.

## 11. 확인 문제
1. hex 한 자리는 이진 몇 자리인가?
2. `0xA5`의 십진 값은?
3. `1111₂`에 대응하는 hex digit은?
4. `%X`는 무엇을 출력하는가?
5. `0x1234` 표기만 보고 메모리 byte 순서를 알 수 있는가?
<details><summary>정답</summary>
1. 4자리. 2. 165. 3. F. 4. unsigned 값을 대문자 16진수로 표시한다. 5. 없다.
</details>

## 12. 핵심 정리
16진 한 자리는 이진 네 자리다. 표기는 값을 읽는 방법이며 메모리 배치가 아니다.

## 13. 다음 Step
[Step 3-4. C의 10진·8진·16진 정수 리터럴](3-4-integer-literals.md)

## 14. 참고 자료
- [WG14 N1570](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf): 6.4.4.1, 7.21.6.1. C11 공개 초안이며 C17 공통 규칙의 참고 자료다.
- [cppreference: integer constants](https://en.cppreference.com/w/c/language/integer_constant)
- [cppreference: `printf`](https://en.cppreference.com/w/c/io/fprintf)
