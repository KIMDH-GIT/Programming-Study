# Step 3-5. N-bit unsigned 표현과 범위

N개의 **값 bit**를 가진 unsigned 정수 모형과 실제 C 정수형의 한계를 연결한다.

## 1. 학습 목표
- N value bits의 범위 `0 ~ 2^N-1`을 유도한다.
- unsigned modulo `2^N`을 설명한다.
- 저장 bit, 값 bit, padding을 구별한다.
- 실제 범위는 한계 매크로로 확인한다.

## 2. 선수 지식
[3-1](3-1-bit-byte-place-value.md)의 저장 단위와 [3-4](3-4-integer-literals.md)의 unsigned 상수를 사용한다.

## 3. 핵심 개념
N value bits의 자릿값 합 최댓값은 `1+2+...+2^(N-1)=2^N-1`이다.

```text
[8 value-bit unsigned 모형]
00000000 = 0
00000001 = 1
11111111 = 255
범위: 0 ~ 2^8-1
```

**[C 표준]** unsigned 정수형은 순수 이진 표현을 사용하고 산술은 범위 크기를 법으로 한다. 여기서 N은 padding을 제외한 값 bit 수다. `unsigned char`에는 padding이 없으므로 그 값 bit 수는 `CHAR_BIT`이고 `UCHAR_MAX`는 `2^CHAR_BIT-1`이다. 다른 unsigned 정수형에는 padding이 있을 수 있으므로 `sizeof(T)*CHAR_BIT`를 N으로 단정하지 않는다.

## 4. 문법
`UCHAR_MAX`, `UINT_MAX`는 `<limits.h>`의 구현 한계다. `%u`에는 `(unsigned int)UCHAR_MAX` 또는 `UINT_MAX`를 전달한다.

## 5. 최소 코드 예제
```c
#include <limits.h>
#include <stdio.h>
int main(void)
{
    printf("CHAR_BIT=%d\n", (int)CHAR_BIT);
    printf("UCHAR_MAX=%u\n", (unsigned int)UCHAR_MAX);
    printf("UINT_MAX=%u\n", UINT_MAX);
    printf("UINT_MAX+1U=%u\n", UINT_MAX + 1U);
    return 0;
}
```
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic unsigned_range.c -o unsigned_range && ./unsigned_range
```

## 6. 코드 해석
첫 세 줄은 구현 특성을 관찰한다. 마지막 결과 0은 `unsigned int`의 modulo 규칙이다. 특정 최댓값 숫자는 고정하지 않는다.

## 7. 내부 동작
컴파일러는 대상의 값 bit와 padding을 정하고 헤더가 한계를 제공한다. CPU 폭과 C 자료형 폭은 같은 개념이 아니다. 유효한 값 연산과 임의 저장 패턴 관찰도 구별한다.

## 8. 자주 하는 실수
- N을 전체 저장 bit 수라고 정의한다.
- 모든 `unsigned int`가 32 bit라고 한다.
- 모든 unsigned 저장 패턴이 정상값이라고 일반화한다.
- `UCHAR_MAX`를 고정 255라고 한다.
- `^`를 C의 거듭제곱 연산자로 생각한다.

## 9. 필수 실습
[README](../../exercises/03-integer-representation/3-5/README.md)에 따라 한계 매크로를 출력하고 4-bit·8-bit unsigned 범위를 종이에서 계산한다.

## 10. 추가 실습
- ★ N=3 범위와 패턴표
- ★★ `UINT_MAX+1U` 관계 설명
- ★★★ 저장 bit와 값 bit 차이 조사

## 11. 확인 문제
1. N value bits의 범위는 무엇인가?
2. 8 value bits의 최댓값은 무엇인가?
3. `unsigned int`에 padding이 가능한가?
4. `unsigned char`의 값 bit 수는 무엇인가?
5. 실제 `UINT_MAX`는 어디서 확인하는가?
<details><summary>정답</summary>
1. `0~2^N-1`. 2. 255. 3. 가능하다. 4. `CHAR_BIT`. 5. `<limits.h>`.
</details>

## 12. 핵심 정리
unsigned 범위는 값 bit 수로 계산한다. 실제 형의 N을 저장 크기만으로 추측하지 않고 한계 매크로를 읽는다.

## 13. 다음 Step
[Step 3-6. 2의 보수 모델과 C17 signed 표현](3-6-twos-complement-c17-signed-representations.md)

## 14. 참고 자료
- [WG14 N1570](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf): 6.2.5, 6.2.6.2. C11 공개 초안이며 C17 공통 규칙 참고 자료다.
- [GCC Integers](https://gcc.gnu.org/onlinedocs/gcc/Integers-implementation.html)
