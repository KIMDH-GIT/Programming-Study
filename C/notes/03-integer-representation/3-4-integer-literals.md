# Step 3-4. C의 10진·8진·16진 정수 리터럴

기준은 C17이다. C 표준 용어는 정수 상수(integer constant)이며 진법, 값, 후보 자료형을 함께 읽는다.

## 1. 학습 목표
- `10`, `012`, `0x0A`의 진법을 구별한다.
- 선행 0의 8진 의미를 설명한다.
- 진법과 suffix에 따른 후보 자료형 차이를 안다.
- C17에는 `0b` 정수 상수가 없음을 안다.

## 2. 선수 지식
[3-3](3-3-hexadecimal-binary.md)과 Part 2의 정수형·접미사를 사용한다.

## 3. 핵심 개념
```text
10   : 십진 10
012  : 팔진 12 = 십진 10
0x0A : 십육진 A = 십진 10
```
`010`은 10이 아니라 8이고 `08`은 올바른 8진 정수 상수가 아니다. C17에는 표준 `0b1010` 문법이 없다.

suffix가 없을 때 십진 상수 후보는 `int`, `long`, `long long` 순이다. 8진·16진은 그 사이에 대응 unsigned 형도 후보로 들어간다. `U`는 unsigned 후보, `L`은 long 후보, `LL`은 long long 후보를 선택하게 한다. 첫 번째로 값을 표현할 수 있는 후보가 형이므로 `U`가 언제나 정확히 `unsigned int`, `L`이 고정 폭을 뜻하지는 않는다.

`-10`은 음수 정수 상수 한 토큰이 아니라 양의 정수 상수 `10`에 단항 `-`를 적용한 식이다.

## 4. 문법
| 표기 | 의미 |
|---|---|
| `42` | 십진 |
| `052` | 8진 |
| `0x2A` | 16진 |
| `42U`, `42UL`, `42LL` | unsigned·길이 후보 suffix |
| `%o`, `%x` | unsigned 값을 8진·16진으로 출력 |

## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void)
{
    int decimal = 10;
    int octal = 012;
    int hexadecimal = 0x0A;
    unsigned int value = 10U;
    printf("%d %d %d\n", decimal, octal, hexadecimal);
    printf("octal=%o hex=%X\n", value, value);
    return 0;
}
```
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic integer_literals.c -o integer_literals && ./integer_literals
```

## 6. 코드 해석
첫 세 객체의 값은 모두 10이다. `%o`, `%X`는 `value`를 각각 12, A로 표시한다. 출력 형식은 객체의 값을 바꾸지 않는다.

## 7. 내부 동작
컴파일러는 토큰의 접두사·suffix와 구현 한계를 사용해 값과 형을 정한다. 표기 문자열이 객체에 그대로 저장된다고 가정하지 않는다. GCC 확장이 있어도 `-std=c17 -Wpedantic` 기준과 분리한다.

## 8. 자주 하는 실수
- 선행 0을 장식으로 쓴다.
- `08`을 유효한 십진 8로 생각한다.
- C17에 `0b`가 있다고 한다.
- unsuffixed decimal이 unsigned 후보를 가진다고 한다.
- suffix가 정확한 bit 폭을 보장한다고 생각한다.

## 9. 필수 실습
[README](../../exercises/03-integer-representation/3-4/README.md)에 따라 같은 값 42를 `42`, `052`, `0x2A`로 선언·출력하고 형 선택 규칙을 기록한다.

## 10. 추가 실습
- ★ `010`과 `10` 비교
- ★★ `U`, `L`, `LL` 후보표 작성
- ★★★ 현재 한계 매크로와 큰 상수 후보 비교

## 11. 확인 문제
1. `012`의 십진 값은 무엇인가?
2. `08`은 유효한 C17 정수 상수인가?
3. C17이 제공하는 표준 이진 정수 접두사는 무엇인가?
4. 접미사 없는 십진 상수의 후보형 순서는 무엇인가?
5. `-1`은 한 정수 상수 토큰인가?
<details><summary>정답</summary>
1. 10. 2. 아니다. 3. 없다. 4. `int`, `long`, `long long`. 5. 아니다.
</details>

## 12. 핵심 정리
접두사는 진법, suffix는 후보 자료형에 관여한다. 값·형·표기·객체 표현을 구별한다.

## 13. 다음 Step
[Step 3-5. N-bit unsigned 표현과 범위](3-5-n-bit-unsigned-representation.md)

## 14. 참고 자료
- [WG14 N1570](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf): 6.4.4.1. C11 공개 초안이며 C17 공통 규칙 참고 자료다.
- [cppreference: integer constants](https://en.cppreference.com/w/c/language/integer_constant)
