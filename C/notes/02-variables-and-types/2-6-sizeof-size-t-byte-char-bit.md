# 2-6. `sizeof`, `size_t`, byte와 `CHAR_BIT`

기준은 C17 호스트 환경이다. `sizeof`의 결과는 비트 수가 아니라 **C byte 수**다. 한 C byte에 몇 비트가 있는지는 `<limits.h>`의 `CHAR_BIT`로 확인한다. 이 Step은 배열, 포인터, 조건문, 반복문, 동적 메모리를 요구하지 않는다.

## 1. 학습 목표

- 형 이름과 변수에 `sizeof`를 적용한다.
- `sizeof`가 함수가 아닌 연산자이며 결과형이 `size_t`임을 설명한다.
- `<stddef.h>`의 `size_t`로 크기를 저장하고 `%zu`로 출력한다.
- `sizeof(char) == 1`과 `CHAR_BIT >= 8`의 의미를 구분한다.
- 객체의 저장 비트 수와 값을 표현하는 비트 수가 다를 수 있음을 설명한다.

## 2. 선수 지식

- [Part 2 학습 순서](../../C_CURRICULUM.md#part-2-변수와-자료형)의 선언·초기화와 정수형 구분.
- [2-5. `float`, `double`, `long double`](2-5-floating-types.md)의 자료형과 표현 능력.
- [표준 헤더](../01-program-structure/1-2-include-standard-headers.md), [출력](../01-program-structure/1-4-printf-multiline-output.md), [빌드와 실행](../00-compilation/0-7-build-and-run.md).

`int count = 7;`은 정수형 변수의 선언·초기화다. 이번에는 그 값 `7`이 아니라 **그 형의 객체가 차지하는 크기**를 묻는다. `%d`는 `int` 출력용, `%zu`는 `size_t` 출력용 서식으로 사용한다. 전체 출력 서식 체계는 Part 5에서 배운다.

## 3. 핵심 개념

### 크기를 묻는 연산자

**[C 표준]** `sizeof`는 피연산자의 형으로부터 객체의 크기를 C byte 단위로 구하는 연산자다. `sizeof(int)`는 `int` 객체의 크기이고 `sizeof count`도 `count`의 형이 `int`이므로 같은 결과다. 변수에 든 값이 `7`인지 `700`인지에 따라 크기가 달라지지 않는다.

이번에 사용하는 기본 자료형에 대한 `sizeof` 결과는 정수 상수 식이다. 피연산자 식은 평가되지 않으므로 변수의 값을 실제로 읽어서 크기를 계산하지 않는다. 나중에 배우는 가변 길이 배열형에는 평가와 관련된 예외가 있으므로 "`sizeof`는 언제나 실행 시 평가를 전혀 하지 않는다"고 일반화하지 않는다.

### `size_t`와 `%zu`

**[C 표준]** `size_t`는 `sizeof` 결과의 형인 **부호 없는 정수형의 이름**이다. `<stddef.h>`에서 이 이름을 제공한다. 새 종류의 숫자나 함수가 아니라 구현이 선택한 적절한 unsigned 정수형에 붙인 이름이다. 별칭을 만드는 문법은 이후 `typedef` Step에서 다룬다.

`size_t`가 항상 `unsigned int`, 항상 `unsigned long`, 항상 64비트라는 보장은 없다. 그러므로 `%u`나 `%lu`를 추측해서 고르지 않고 C99부터 제공되는 `%zu`로 출력한다. `z`는 크기형에 맞추는 길이 수정자, `u`는 부호 없는 십진 출력이다. `sizeof` 연산자 자체에는 헤더가 필요 없지만 `size_t`라는 이름을 직접 쓸 때는 이를 제공하는 헤더를 포함한다. 이 자료는 `<stddef.h>`를 명시한다.

### C byte, bit, octet

- **bit:** 0 또는 1을 나타내는 이진 정보 단위다.
- **C byte:** C 실행 환경에서 기본 문자 집합의 문자를 담을 수 있는 주소 지정 가능한 저장 단위다. `sizeof`의 단위이기도 하다.
- **`CHAR_BIT`:** `<limits.h>`가 제공하는 한 byte의 비트 수다. C17은 적어도 8을 요구하지만 정확히 8이라고 고정하지 않는다.
- **octet:** 정확히 8비트를 뜻한다. `CHAR_BIT == 8`인 환경에서 한 C byte가 한 octet과 같다.

**[C 표준]** `sizeof(char)`, `sizeof(signed char)`, `sizeof(unsigned char)`는 모두 1이다. 이 1은 **한 C byte**라는 뜻이지 항상 8비트라는 뜻이 아니다. 예를 들어 `CHAR_BIT`가 16인 적합한 구현에서는 `char` 객체 하나가 한 C byte이면서 16비트의 저장 공간을 가진다.

`sizeof(int)`가 4이고 `CHAR_BIT`가 8인 구현이라면 `int` 객체의 저장 공간은 32비트다. 수학적으로 **C byte 수 × byte당 비트 수**로 계산한다. 부호 비트나 패딩 비트가 있을 수 있어 이 수를 양의 값 표현에 쓰는 비트 수라고 부르면 안 된다. 실제 정수 범위는 다음 Step의 `<limits.h>`로 확인한다.

## 4. 문법

| 표현 | 의미 |
|---|---|
| `sizeof(int)` | 형 이름에는 괄호가 필요하다. |
| `sizeof count` | 변수 식에 적용한다. |
| `sizeof(count)` | 식에 괄호를 써도 된다. 함수 호출이 되는 것은 아니다. |
| `size_t bytes = sizeof(double);` | 크기를 결과형과 같은 형의 변수에 저장한다. |
| `printf("%zu\n", bytes);` | `size_t`를 부호 없는 십진수로 출력한다. |
| `printf("%d\n", (int)CHAR_BIT);` | `CHAR_BIT`를 표현 가능한 `int`로 변환해 `%d`로 출력한다. |

`sizeof int`는 형 이름에 괄호가 없어 올바르지 않다. `sizeof(void)`와 함수에 대한 `sizeof`도 표준 C의 올바른 사용이 아니다. 일부 컴파일러 확장을 C17 규칙으로 취급하지 않는다.

**[C 표준]** 기본 정수형 크기에는 다음 관계가 성립한다.

```text
1 == sizeof(char) <= sizeof(short) <= sizeof(int)
  <= sizeof(long) <= sizeof(long long)
```

이 관계는 반드시 단계마다 커진다는 뜻이 아니다. 특히 `sizeof(int) == 4`나 `sizeof(long) == 8`은 C의 보편적인 요구 사항이 아니다. 부동소수점형의 정밀도 역시 크기 하나만으로 결정할 수 없다.

## 5. 최소 코드 예제

`type_sizes.c`에 저장한다.

```c
#include <stddef.h>
#include <stdio.h>
#include <limits.h>

int main(void)
{
    int count = 7;
    size_t count_bytes = sizeof count;

    printf("sizeof(char) = %zu C byte\n", sizeof(char));
    printf("CHAR_BIT = %d bits per C byte\n", (int)CHAR_BIT);
    printf("sizeof(int) = %zu C bytes\n", sizeof(int));
    printf("sizeof count = %zu C bytes\n", count_bytes);
    printf("sizeof(double) = %zu C bytes\n", sizeof(double));
    return 0;
}
```

**[GCC/Linux 구현]** 파일이 있는 디렉터리에서 실행한다.

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic type_sizes.c -o type_sizes && ./type_sizes
printf 'status=%s\n' "$?"
```

흔한 x86-64 GNU/Linux 환경의 출력 예시는 다음과 같다. 첫 번째 값 1과 두 `int` 크기가 같다는 관계는 표준의 사실이다. 8, 4, 8이라는 구체적인 측정값은 해당 구현의 관찰이다.

```text
sizeof(char) = 1 C byte
CHAR_BIT = 8 bits per C byte
sizeof(int) = 4 C bytes
sizeof count = 4 C bytes
sizeof(double) = 8 C bytes
```

## 6. 코드 해석

| 구성 | 해석 |
|---|---|
| `<stddef.h>` | `size_t`라는 형 이름을 제공한다. |
| `<stdio.h>` | `printf` 선언을 제공한다. |
| `<limits.h>` | 이번에는 `CHAR_BIT`만 사용한다. 정수 범위는 2-7에서 다룬다. |
| `int count = 7;` | 크기를 관찰할 `int` 객체를 선언하고 초기화한다. |
| `size_t count_bytes = sizeof count;` | `count`의 값이 아니라 형의 크기를 구해 저장한다. |
| `sizeof(char)` 출력 | 값 1을 `%zu`로 출력한다. 상수처럼 보이더라도 결과형은 `size_t`다. |
| `CHAR_BIT` 출력 | 한 C byte의 비트 수를 `%d`로 출력한다. |
| 두 `int` 관련 출력 | 형에 적용한 결과와 그 형의 변수에 적용한 결과를 비교한다. |
| `sizeof(double)` 출력 | 표현 정밀도가 아닌 객체 크기를 관찰한다. |
| `return 0;` | 성공 종료를 나타낸다. |

## 7. 내부 동작

1. **[C 표준·컴파일러]** 컴파일러는 선언으로 `count`의 형을 안다. 기본형의 `sizeof`는 값이나 실제 메모리를 검사하지 않고 형 정보로 결정할 수 있다. `sizeof`를 실행하기 위해 라이브러리 함수를 호출해야 한다는 규칙은 없다.
2. **[구현·ABI]** 대상 구현의 자료형 배치 규칙이 구체적인 크기를 정한다. 같은 CPU 계열이라도 ABI와 컴파일 대상이 다르면 `long` 등의 크기가 다를 수 있다. "64비트 OS이므로 모든 정수형과 `size_t`가 8바이트"라는 추론은 틀리다.
3. **[C 표준·메모리]** `sizeof`는 객체 표현에 포함된 패딩도 센다. 객체를 저장하는 데 필요한 공간과 정수의 값 비트 수, 부동소수점의 유효 자릿수는 서로 다른 정보다.
4. **[구현·CPU]** 최적화된 코드에서는 알려진 크기를 바로 출력 함수에 전달할 수 있다. `sizeof` 때문에 CPU가 RAM을 스캔하거나 `count`를 읽을 필요는 없다. 소스의 모든 변수가 실제 메모리에 별도 자리를 가져야 하는 것도 아니다.
5. **[라이브러리·OS]** `%zu`가 크기 값을 문자로 변환하고 stdout에 쓴다. 이 출력은 객체 크기이지 프로세스 전체 메모리 사용량, 실행 파일 크기, OS의 할당 단위가 아니다.

## 8. 자주 하는 실수

- **`sizeof(char) == 1`이니 1비트라고 생각한다.** 결과 단위는 C byte다.
- **byte는 무조건 8비트라고 쓴다.** 흔한 구현에서는 그렇지만 C에서는 `CHAR_BIT`를 확인해야 한다.
- **`sizeof` 결과를 `%d`로 출력한다.** 결과형은 `size_t`다. 서식 인수형 불일치는 정의되지 않은 동작이며 경고가 없어도 옳아지지 않는다.
- **`size_t`가 이 컴퓨터에서 `unsigned long`이니 `%lu`를 보편 규칙으로 삼는다.** 다른 구현에서는 달라질 수 있으므로 `%zu`를 쓴다.
- **변수 값이 커지면 `sizeof`도 커진다고 생각한다.** 기본형 객체의 크기는 값과 무관하다.
- **`sizeof(int)`만 보고 `int`의 최댓값을 단정한다.** 부호와 패딩 여부가 필요하며 범위는 `<limits.h>`로 확인한다.
- **`sizeof('A')`가 `sizeof(char)`라고 생각한다.** C에서 `'A'` 같은 보통 정수 문자 상수의 형은 `int`다. `char` 크기를 재려면 `sizeof(char)`나 `char` 변수를 사용한다.

## 9. 필수 실습

### 기초 A. 크기와 단위 함께 출력하기

- **목적:** `sizeof` 결과형과 C byte의 의미를 확인한다.
- **해야 할 일:** [실습 README](../../exercises/02-variables-and-types/2-6/README.md)에 따라 `type_sizes.c`를 작성한다. `char`, `short`, `int`, `long`, `long long`, `float`, `double`, `long double`의 크기와 `CHAR_BIT`를 한 줄씩 출력한다. `int` 변수 하나의 크기를 `size_t` 변수에 저장해 별도 줄로 출력한다.
- **사용할 개념:** `sizeof(형)`, `sizeof 변수`, `size_t`, `%zu`, `%d`, `CHAR_BIT`.
- **예상 관찰 결과:** `char` 크기는 1, `CHAR_BIT`는 8 이상, `int`형과 그 변수의 크기는 같다. 다른 크기는 환경별 측정값이다.
- **확인 포인트:** 단위를 C byte와 bit로 구별했는가? 출력한 크기를 고정된 보편 정답으로 만들지 않았는가?

## 10. 추가 실습

### 응용 B. 값 변경과 크기 비교

- **목적:** 값과 형의 크기를 분리한다.
- **해야 할 일:** `value_and_size.c`에서 `int` 변수 두 개를 각각 `7`, `700`으로 초기화하고 값은 `%d`, 크기는 `%zu`로 출력한다. 결과가 같은 이유를 적는다.
- **사용할 개념:** 선언·초기화, 값 출력, `sizeof`의 피연산자형.
- **예상 관찰 결과:** 값은 다르지만 크기는 같다. 두 값은 C17의 최소 `int` 범위 안에 있다.
- **확인 포인트:** 십진수로 출력되는 글자 수와 저장 공간을 구별했는가?

### 선택 도전 C. 저장 비트 수 해석

- **목적:** byte와 bit의 관계를 수치로 설명한다.
- **해야 할 일:** 필수 실습에서 얻은 `char`, `int`, `double`의 크기를 기록하고, 각각에 `CHAR_BIT`를 곱한 저장 비트 수를 종이나 계산기로 구한다. `CHAR_BIT`가 16이고 `sizeof(int)`가 2인 가상의 적합한 구현도 같은 방법으로 해석한다. 추가 C 코드는 필요 없다.
- **사용할 개념:** C byte, bit, `CHAR_BIT`, 패딩과 값 표현의 구분.
- **예상 관찰 결과:** 가상 구현의 `int` 저장 공간은 32비트다. 한 C byte가 8비트인 구현과 크기 숫자가 달라도 총 저장 비트 수는 같을 수 있다.
- **확인 포인트:** 총 저장 비트 수만으로 정수 최댓값이나 부동소수점 정밀도를 단정하지 않았는가?

## 11. 확인 문제

1. **빈칸:** `sizeof(double)` 결과형은 무엇이며 출력 서식은 무엇인가?
2. **참·거짓:** `sizeof(char) == 1`은 모든 C17 구현에서 `char`가 정확히 8비트임을 뜻한다.
3. **계산:** `CHAR_BIT`가 16이고 `sizeof(long)`이 4라면 `long` 객체 저장 공간은 몇 비트인가? 모두 값 비트라고 할 수 있는가?
4. **오류 찾기:** `size_t bytes = sizeof int;`의 문법 오류를 고쳐라. `printf("%d\n", bytes);`도 올바르게 고쳐라.
5. **결과 관계 예측:** `int a = 1;`과 `int b = 1000;`에서 `sizeof a`와 `sizeof b` 중 어느 것이 큰가?
6. **설명:** `sizeof(double)`과 `sizeof(long double)`이 같으면 두 형의 정밀도도 반드시 같다고 결론 내려도 되는가?

<details>
<summary>정답과 해설 펼치기</summary>

1. `size_t`이며 `%zu`다. 이름을 직접 쓸 때 `<stddef.h>`를 포함한다.
2. 거짓이다. 한 C byte라는 뜻이며 한 byte의 비트 수는 `CHAR_BIT`다. C17은 `CHAR_BIT >= 8`을 보장한다.
3. 64비트다. 부호 비트와 패딩 등이 있을 수 있으므로 전부 값을 나타내는 비트라고 할 수 없다.
4. `size_t bytes = sizeof(int);`와 `printf("%zu\n", bytes);`로 고친다. 형 이름에는 괄호가 필요하고 출력형도 맞춰야 한다.
5. 같다. 둘 다 같은 `int`형의 객체이며 저장된 수의 십진 자릿수와 무관하다.
6. 안 된다. 객체 크기에는 패딩도 포함되며 형식과 유효 자릿수를 알려 주지 않는다. 실제 정밀도는 2-8에서 `<float.h>`로 확인한다.

</details>

## 12. 핵심 정리

- `sizeof`는 연산자이며 C byte 수를 `size_t`로 만든다.
- `size_t`의 구체적인 unsigned 정수형을 추측하지 않고 `%zu`를 쓴다.
- `sizeof(char) == 1`은 한 C byte를 뜻한다. 그 비트 수인 `CHAR_BIT`는 8 이상이지 반드시 8은 아니다.
- 크기와 값의 범위, 저장 공간과 유효 자릿수를 구별한다.

## 13. 다음 Step

[2-7. `<limits.h>`와 정수 범위](../../C_CURRICULUM.md#part-2-변수와-자료형)에서 `sizeof`만으로는 알 수 없는 정수 최솟값·최댓값을 확인한다. 링크는 전체 교육과정의 Part 2 목록으로 연결된다.

## 14. 참고 자료

- [WG14 N1570 공개 초안](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf): 3.6 byte, 5.2.4.2.1 `CHAR_BIT`, 6.2.6 객체 표현, 6.5.3.4 `sizeof`, 7.19 `size_t`, 7.21.6.1 `z` 출력 길이 수정자. C11 공개 초안이며 C17 최종 원문이 아니라 공통 규칙의 공개 참고 자료다.
- [ISO/IEC 9899:2018](https://www.iso.org/standard/74528.html): C17 공식 표준의 서지 정보.
- [GCC: Storage Layout](https://gcc.gnu.org/onlinedocs/gccint/Storage-Layout.html): 대상의 byte당 비트 수와 자료형 저장 배치를 기술하는 GCC 구현 문서. C의 보장과 대상 설정을 구분할 때 참고한다.
