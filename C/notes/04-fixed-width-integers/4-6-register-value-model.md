# 4-6. `uint32_t register_value`와 레지스터 모델

전자 장치 문서에는 “32-bit register”처럼 폭이 계약으로 주어지는 값이 자주 등장한다. 이번 Step은 실제 하드웨어에 접근하지 않고 `uint32_t` 변수 하나로 그 **값 모델**만 표현한다.

## 1. 학습 목표

- 32-bit 레지스터 값을 `uint32_t`로 모델링하는 이유를 설명한다.
- 값 모델과 실제 memory-mapped I/O 접근을 구별한다.
- 16진 표기가 레지스터 문서와 잘 맞는 이유를 설명한다.
- `UINT32_C`와 `PRIx32`를 함께 사용한다.
- exact-width 형만으로 해결되지 않는 하드웨어 계약을 말한다.

## 2. 선수 지식

Step 4-3의 `uint32_t`, Step 4-5의 `UINT32_C`, `PRIx32`, Part 3의 16진 표기를 사용한다. 비트 마스크와 shift는 Part 21, 포인터는 Part 14 이후, `volatile`과 memory-mapped I/O는 Part 31에서 배운다.

## 3. 핵심 개념

### 3.1 레지스터 값의 폭

장치 문서가 레지스터를 32 bit라고 정했다면 그 값을 저장하는 모델에는 `uint32_t`가 잘 맞는다.

- 정확히 32 bit다.
- 음수가 없는 0부터 2<sup>32</sup>-1 범위다.
- 32개의 상태 bit를 값으로 표현할 수 있다.
- 8자리 16진수와 대응시켜 문서의 패턴을 읽기 쉽다.

단, 이는 `uint32_t`가 제공되는 구현에서의 이야기다. 실제 대상 C 구현이 exact 32-bit 정수형을 제공하는지 먼저 확인해야 한다.

### 3.2 값 모델과 실제 장치 접근

```c
uint32_t register_value = UINT32_C(0xA5A50001);
```

이 선언은 보통의 자동 변수를 만든다. 실제 장치 주소를 읽거나 쓰지 않는다. 다음은 별도의 주제다.

- 장치 주소와 포인터
- `volatile` 접근
- 읽기·쓰기 부작용
- read-modify-write 규약
- 동시성·atomicity

이 내용은 이후 Embedded/System C Part에서 다룬다. 지금은 문서에 적힌 32-bit 값을 C 값으로 정확히 보관하고 표시하는 단계다.

### 3.3 16진수와 32 bit

16진수 한 자리는 4 bit에 대응하므로 32-bit 값은 8자리 16진수로 적을 수 있다.

```text
0xA5A50001
 A5 A5 00 01  → 8개의 16진 숫자 → 32 bit
```

앞쪽 0도 폭을 보여 줄 때 의미가 있다. `PRIx32`는 기본적으로 필요한 숫자만 출력하므로, 8자리 폭을 맞추려면 `"%08" PRIx32`처럼 최소 필드 폭과 0 채움을 조합할 수 있다.

### 3.4 정확한 폭이 해결하지 않는 것

`uint32_t`는 값의 폭과 범위를 해결한다. 하지만 레지스터 각 bit의 의미, 쓰기 가능한 bit, 예약 bit, 접근 순서, byte 순서, 실제 주소는 장치 문서가 정한다. 자료형 하나만으로 하드웨어 규약을 대신할 수 없다.

## 4. 문법

```c
#include <inttypes.h>
#include <stdio.h>

uint32_t register_value = UINT32_C(0xA5A50001);

printf("register=0x%08" PRIx32 "\n", register_value);
```

`08`은 최소 8자리, 빈 자리를 0으로 채우라는 `printf` 형식 요소다. `PRIx32`가 실제 변환 문자와 필요한 길이 수정자를 제공한다.

## 5. 최소 코드 예제

`register_model.c`:

```c
#include <inttypes.h>
#include <stdio.h>

int main(void)
{
    uint32_t reset_value = UINT32_C(0x00000000);
    uint32_t register_value = UINT32_C(0xA5A50001);

    printf("reset=0x%08" PRIx32 "\n", reset_value);
    printf("register=0x%08" PRIx32 "\n", register_value);
    printf("range=0..%" PRIu32 "\n", UINT32_MAX);
    return 0;
}
```

이 코드는 평범한 변수의 값을 출력할 뿐 실제 하드웨어 레지스터를 접근하지 않는다.

## 6. 코드 해석

1. `<inttypes.h>`를 통해 `uint32_t`, `UINT32_C`, `UINT32_MAX`, `PRI` 매크로를 사용한다.
2. `reset_value`는 모든 값 bit가 0인 모델이다.
3. `register_value`는 문서에서 읽은 32-bit 예시 패턴을 담는다.
4. `0x%08` 뒤에 `PRIx32`가 이어져 8자리 소문자 16진 출력을 만든다.
5. `UINT32_MAX`는 모델이 표현 가능한 최댓값이다.
6. 대입과 출력만 사용하며 bit 추출·변경은 아직 하지 않는다.

## 7. 내부 동작

**[C 값]** `uint32_t` 객체는 0부터 2<sup>32</sup>-1까지의 수학적 값을 저장한다. 16진 리터럴과 10진 출력은 같은 값을 다른 문자 표기로 보여 준다.

**[메모리]** 자동 변수는 실행 환경이 정한 저장 위치에 놓일 수 있다. 컴파일러는 레지스터에만 두거나 최적화할 수도 있다. 소스 변수 이름의 `register_value`는 CPU 또는 장치 레지스터 배치를 강제하지 않는다.

**[하드웨어]** 실제 장치 레지스터는 읽기나 쓰기 자체가 동작을 일으킬 수 있다. 이 예제의 평범한 객체에는 그런 의미가 없다.

**[출력]** `printf`는 숫자값을 16진 문자로 변환한다. 출력된 왼쪽에서 오른쪽 문자 순서를 메모리 byte 순서로 해석하지 않는다.

## 8. 자주 하는 실수

- 변수 이름에 `register`가 들어가면 실제 장치 레지스터에 연결된다고 생각한다.
- `uint32_t`를 쓰기만 하면 `volatile`이나 주소 지정이 자동으로 해결된다고 생각한다.
- 16진 출력 문자열 순서를 메모리 byte 순서와 같다고 단정한다.
- 32-bit 값을 항상 8 C byte라고 읽거나, `sizeof`의 단위를 bit로 착각한다.
- 아직 배우지 않은 bit mask와 shift를 필수 실습에 넣는다.
- 예약 bit와 쓰기 부작용을 장치 문서 없이 임의로 정한다.

## 9. 필수 실습

### 32-bit 레지스터 값 카드

- **목적:** 장치와 분리된 32-bit 값 모델을 만들고 8자리 16진수로 표시한다.
- **해야 할 일:** reset 값과 예시 register 값을 `uint32_t`로 선언하고 `UINT32_C`로 초기화한다. 두 값을 `0x` 접두 표시와 8자리 0 채움으로 출력한다.
- **사용할 개념:** `uint32_t`, `UINT32_C`, `UINT32_MAX`, `PRIx32`, 16진 네 bit 묶음.
- **예상 관찰 결과:** `0x00000000`과 `0xa5a50001`이 표시된다.
- **확인 포인트:** 이 프로그램이 실제 하드웨어 접근이 아니라는 설명을 남겼는가?

자세한 절차는 [실습 README](../../exercises/04-fixed-width-integers/4-6/README.md)에 있다.

## 10. 추가 실습

- ★ **기초:** `0x12345678`을 4-bit 묶음 8개로 나누어 적는다.
- ★★ **응용:** 값 `0x0000002A`를 16진과 `PRIu32` 10진으로 각각 출력한다.
- ★★★ **도전:** 정확한 폭, byte 순서, 접근 부작용을 서로 다른 계약 세 줄로 정리한다. 실제 포인터·`volatile` 코드는 쓰지 않는다.

## 11. 확인 문제

1. `uint32_t register_value` 선언만으로 실제 장치 레지스터에 접근하는가?
2. 32-bit 값을 같은 폭으로 적으려면 16진 숫자가 몇 자리 필요한가?
3. `0x%08`과 `PRIx32`는 각각 어떤 역할을 하는가?
4. `uint32_t`가 해결하는 계약과 해결하지 않는 계약을 하나씩 말하라.
5. 출력된 16진 문자 순서로 메모리 endianness를 알 수 있는가?

<details>
<summary>정답과 해설</summary>

1. 아니다. 평범한 C 객체를 만든다.
2. 8자리다.
3. 전자는 `0x`와 최소 8자리 0 채움을, 후자는 `uint32_t`에 맞는 16진 변환 서식 조각을 제공한다.
4. 정확한 32-bit 값 폭은 해결한다. 실제 주소·접근 부작용·byte 순서 등은 해결하지 않는다.
5. 알 수 없다. `printf`가 값에서 문자를 생성한 결과일 뿐 객체 표현을 관찰한 것이 아니다.

</details>

## 12. 핵심 정리

`uint32_t`는 32-bit 레지스터의 **값**을 모델링하기에 알맞다. `UINT32_C`로 상수를 만들고 `PRIx32`로 이식성 있게 출력할 수 있다. 그러나 평범한 변수는 실제 하드웨어 레지스터가 아니며 주소, `volatile`, bit 조작, 접근 규약은 이후 Part에서 별도로 배운다.

## 13. 다음 Step

[Step 4-7. Part 4 종합 복습](4-7-part-4-review.md)

## 14. 참고 자료

- [WG14 N2176, C17 ballot draft](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 7.20의 폭 기반 정수형과 7.8의 형식 매크로를 확인한다.
- [cppreference: Fixed width integer types](https://en.cppreference.com/w/c/types/integer.html): `uint32_t`, `UINT32_C`, `PRIu32`, `PRIx32`를 확인한다.
- [GCC: Volatiles](https://gcc.gnu.org/onlinedocs/gcc/Volatiles.html): 이후 학습할 구현 관점의 volatile 접근 참고 자료다. 이번 Step의 평범한 값 모델과 혼동하지 않는다.
