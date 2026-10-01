# 5-18. Part 5 종합 복습

Part 5의 중심은 서식 문자를 외우는 것이 아니라 **문자열 속 형식 계약과 실제 인자형을 일치시키는 것**이다. 출력은 값을 전달하고 입력은 저장할 객체의 주소를 전달한다. 입력 뒤에는 `scanf` 반환값으로 성공한 대입 수를 확인하며, 계산 결과는 피연산자형·단위·출력 서식까지 함께 검토한다.

## 1. 학습 목표

- `printf`의 값 인자와 `scanf`의 포인터 인자를 구별한다.
- 정수·부동소수점·문자·문자열·주소·크기에 알맞은 서식을 선택한다.
- `scanf`의 성공, matching failure, 첫 변환 전 input failure를 반환값으로 구별한다.
- buffer 용량에 맞는 `%s` 필드 폭을 계산한다.
- format mismatch의 compiler diagnostic과 C17 Undefined Behavior를 구별한다.
- Part 5에서 배운 제한된 산술로 계산한 값을 올바른 서식과 단위로 출력한다.

## 2. 선수 지식

Step 5-1~5-17 전체와 Part 2~4의 기본형, 범위, `sizeof`, C byte, 고정 폭 정수형의 출력 계약을 사용한다. 조건 분기와 반복은 아직 필수 해결 수단으로 사용하지 않는다.

## 3. 핵심 개념

### 3.1 개념 연결 지도

```text
형식 문자열
├─ printf: 값을 문자로 변환한다
│  ├─ 정수: %d, %u, %x
│  ├─ 부동소수점: %f, %Lf
│  ├─ 문자·문자열: %c, %s
│  └─ 주소·크기: %p, %zu
└─ scanf: 문자를 값으로 변환해 객체에 저장한다
   ├─ 스칼라 객체: 정확한 포인터형을 전달한다
   ├─ 문자열 배열: 용량보다 1 작은 폭을 사용한다
   └─ 반환값: 성공한 대입 수 또는 첫 변환 전 input failure

계산
├─ 정수 /와 %: 몫과 나머지
├─ 실수 비율: 실수 피연산자를 유지
└─ 결과 출력: 값의 형과 단위를 서식에 맞춘다
```

이 세 흐름은 분리되어 있지 않다. 입력으로 값을 얻고, 계산하고, 출력하는 모든 단계에서 형과 계약을 추적해야 한다.

### 3.2 `printf`와 `scanf`의 핵심 차이

| 함수 | 변환 방향 | 추가 인자 | 예 |
|---|---|---|---|
| `printf` | C 값 → 문자 | 출력할 값 | `printf("%d", count)` |
| `scanf` | 문자 → C 값 | 저장할 객체를 가리키는 포인터 | `scanf("%d", &count)` |

`printf`의 가변 인수에는 기본 인수 승격이 적용된다. `float`는 `double`로, `char`와 `short` 계열은 보통 `int` 또는 `unsigned int`로 승격된다. `long double`은 `%Lf`가 필요하다.

`scanf`는 객체에 직접 저장하므로 포인터형이 정확해야 한다. 입력 `%f`는 `float *`, `%lf`는 `double *`, `%Lf`는 `long double *`를 요구한다.

### 3.3 출력 형식 계약

| 목적 | 값형 | 출력 지정 |
|---|---|---|
| signed 10진 | `int` | `%d` |
| unsigned 10진·16진 | `unsigned int` | `%u`, `%x` |
| 일반 부동소수점 | `double` | `%f` |
| 확장 부동소수점 | `long double` | `%Lf` |
| 문자 | 승격된 `int` | `%c` |
| null 종료 문자열 | 첫 문자를 가리키는 포인터 | `%s` |
| 객체 주소 | `void *` | `%p` |
| `sizeof` 결과 | `size_t` | `%zu` |

`%x`는 `0x`를 자동으로 붙이지 않는다. `%s`는 null 문자 또는 출력 정밀도 한계까지 읽는다. `%p`의 구체적인 문자 표현과 기본형 크기는 구현의 영향을 받는다.

### 3.4 입력 형식 계약

| 입력 지정 | 저장 대상 |
|---|---|
| `%d` | `int *` |
| `%u`, `%x` | `unsigned int *` |
| `%f` | `float *` |
| `%lf` | `double *` |
| `%Lf` | `long double *` |
| `%c` | `char *` |
| `%15s` | 적어도 16칸인 `char` 배열의 첫 원소를 가리키는 포인터 |

`char word[16]`에는 최대 15문자와 종료 null 문자가 들어가므로 `%15s`를 사용한다. 폭 없는 `%s`는 목적지 배열의 용량을 알지 못하므로 안전하지 않다.

### 3.5 `scanf` 반환값으로 상태 구별

두 변환을 요청했다고 가정한다.

| 반환값 | 의미 |
|---|---|
| `2` | 두 객체에 모두 대입 성공 |
| `1` | 첫 객체에만 대입 성공. 두 번째 변환은 matching failure 또는 input failure로 대입되지 않음 |
| `0` | 첫 변환부터 입력 문자가 요구 형식과 맞지 않음 |
| `EOF` | 첫 대입 전에 input failure가 발생함. 입력 끝 또는 읽기 오류가 원인일 수 있음 |

matching failure를 일으킨 문자는 입력 stream에 남을 수 있다. `EOF`는 음수인 정수 상수 매크로이며 구체적인 숫자를 하드코딩하지 않는다.

### 3.6 정상 실행과 diagnostic 구별

다음 세 판단은 서로 다르다.

1. **정상 프로그램의 strict build:** 올바른 코드가 `-Werror`에서도 진단 없이 compile되는지 확인한다.
2. **잘못된 예의 diagnostic 관찰:** mismatch 호출은 compile-only로 검사하고 실행하지 않는다.
3. **C17 의미 분류:** format/type 계약 위반이 UB인지 표준 함수 계약으로 판단한다.

compiler가 경고를 냈다는 사실이 UB의 정의는 아니다. 반대로 경고가 없다는 사실도 모든 동적 형식 문자열의 안전을 증명하지 않는다.

### 3.7 제한된 산술과 단위

- `17 / 5`는 정수 몫 3이고 `17 % 5`는 나머지 2다.
- `17.0 / 5.0`은 실수 나눗셈이다.
- 섭씨→화씨 변환은 `celsius * 9.0 / 5.0 + 32.0`처럼 실수 비율을 유지한다.
- 전체 초는 큰 단위의 몫을 구한 뒤 나머지를 다음 단위로 넘긴다.
- BMI는 kg와 m 단위를 사용하고 키가 0보다 커야 한다.

Part 5에서는 안전한 고정값과 초기값으로 계산 구조를 관찰한다. 사용자 입력을 검사해 다른 경로로 분기하는 완성형 프로그램은 이후 제어문 학습과 연결한다.

## 4. 문법

```c
printf("%d %u 0x%x %.2f %c %s %p %zu\n",
       signed_value, unsigned_value, unsigned_value,
       real_value, marker, word,
       (void *)&signed_value, sizeof signed_value);

matched = scanf("%d %x %lf %15s",
                &signed_value, &unsigned_value, &real_value, word);
```

- `printf`에는 각 지정이 요구하는 **값**을 순서대로 전달한다.
- `scanf`에는 각 지정이 요구하는 **포인터**를 전달한다.
- 배열 `word`는 이 호출에서 첫 원소를 가리키는 포인터로 전달되므로 별도의 `&`를 붙이지 않는다.
- `%15s`의 15는 `char word[16]`에 종료 null 문자 한 칸을 남긴다.

## 5. 최소 코드 예제

```c
#include <stdio.h>

int main(void)
{
    int signed_value = -1;
    unsigned int hex_value = 0U;
    double measured = -999.0;
    char marker = 'C';
    char word[16] = "unchanged";
    int matched;
    int written;
    unsigned int code = 42U;
    double celsius = 25.0;
    int total_seconds = 7384;

    matched = scanf("%d %x %lf %15s",
                    &signed_value, &hex_value, &measured, word);

    printf("matched=%d EOF=%d\n", matched, EOF);
    printf("input: signed=%d hex=0x%x measured=%.2f word=%s\n",
           signed_value, hex_value, measured, word);

    written = printf("marker=%c unsigned=%u hex=0x%x\n",
                     marker, code, code);
    printf("written=%d\n", written);

    printf("temperature=%.2f F\n",
           celsius * 9.0 / 5.0 + 32.0);
    printf("time=%d:%02d:%02d\n",
           total_seconds / 3600,
           total_seconds % 3600 / 60,
           total_seconds % 60);
    printf("signed address=%p int bytes=%zu\n",
           (void *)&signed_value, sizeof signed_value);
    return 0;
}
```

정상 입력 예:

```text
7 2a 25.0 C17
```

## 6. 코드 해석

1. 네 입력 객체를 먼저 알려진 값으로 초기화한다. 입력이 일부만 성공해도 초기화되지 않은 값을 읽지 않는다.
2. `%d`, `%x`, `%lf`, `%15s`는 각각 `int *`, `unsigned int *`, `double *`, 16칸 문자 배열과 대응한다.
3. 정상 입력 네 항목이 모두 대입되면 `matched`는 4다.
4. 두 번째 입력이 `nope`처럼 16진 정수와 맞지 않으면 첫 번째 대입만 성공해 `matched`는 1이고 뒤 객체들은 초기값을 유지한다.
5. 첫 대입 전 입력 끝이면 `matched == EOF`다. 프로그램은 실제 `EOF` 값도 함께 출력하므로 특정 음수값을 가정할 필요가 없다.
6. `printf`는 값 인자를 받아 문자로 변환한다. `written`은 `marker=...` 줄에서 성공적으로 쓴 문자 수를 기록한다.
7. 온도식은 실수 비율을 유지하고, 시간식은 정수 몫과 나머지로 단위를 분해한다.
8. `%p`에는 `(void *)&signed_value`, `%zu`에는 `sizeof signed_value`가 대응한다. 주소 표현과 `int` 크기는 구현 관찰값이다.

이 예제는 제어문 없이 정상·부분 실패·EOF 입력의 상태를 **관찰**한다. 실패한 입력을 실제 계산에 사용해도 된다는 뜻은 아니다. 반환값에 따라 경로를 나누는 방법은 조건문을 배운 뒤 완성한다.

## 7. 내부 동작

**[C17 language]** `printf`와 `scanf`의 형식 문자열은 뒤 인자를 해석하는 계약이다. 요구형과 실제형이 맞지 않으면 동작이 정의되지 않을 수 있다. `scanf`는 성공한 변환에 대해서만 객체에 대입한다.

**[compiler]** 문자열 리터럴인 형식은 compiler가 많은 mismatch를 진단할 수 있다. 진단은 계약 검토를 돕지만 language semantics를 대신하지 않는다.

**[ABI]** 가변 인수 호출에서 정수·부동소수점·포인터 인자가 서로 다른 위치나 규칙으로 전달될 수 있다. 형식 불일치가 단순한 “잘못된 표시”로 끝난다고 가정할 수 없다.

**[OS/runtime]** 표준 입력의 EOF 전달 방법과 터미널 buffering은 실행 환경의 영향을 받는다. pipe와 redirection을 사용하면 같은 입력 사례를 재현하기 쉽다.

**[CPU/device]** 이 Part의 표준 입출력과 산술 결과를 설명하는 데 특정 CPU 명령이나 장치 레지스터 지식은 필요하지 않다.

## 8. 자주 하는 실수

- `printf`와 `scanf`의 `%f` 규칙을 같다고 외운다.
  → 출력 `float`는 `double`로 승격되지만 입력은 실제 저장 객체의 포인터형을 요구한다.
  → 출력 `%f`, 입력 `float *`에는 `%f`, 입력 `double *`에는 `%lf`로 구별한다.

- `scanf("%d", value)`처럼 값을 전달한다.
  → 입력 함수가 저장할 객체의 위치를 받지 못한다.
  → 스칼라 객체에는 `&value`를 전달한다.

- `char word[16]`에 `%16s`를 사용한다.
  → 최대 16문자 뒤에 붙을 종료 null 문자 공간이 없다.
  → `%15s`로 한 칸을 남긴다.

- `scanf`가 호출되었으니 모든 입력이 성공했다고 생각한다.
  → 반환값은 성공한 대입 수이고 0, 부분 성공, `EOF`가 가능하다.
  → 기대한 대입 수와 실제 반환값을 비교해 해석한다.

- compiler warning을 실행 결과처럼 취급한다.
  → diagnostic은 번역 도구의 관찰이고 UB는 C17의 의미 분류다.
  → mismatch 예는 compile-only로 확인하고 실행하지 않는다.

- `9 / 5`를 온도 비율로 사용한다.
  → 두 피연산자가 정수라서 먼저 정수 몫 1이 된다.
  → `9.0 / 5.0`처럼 실수 피연산자를 사용한다.

## 9. 필수 실습

정상·부분 실패·EOF 입력을 같은 프로그램으로 재현하고, 여러 출력 서식과 제한된 계산을 연결한다. 별도 format 계약표와 mismatch 분석표도 완성한다. 자세한 절차는 [실습 README](../../exercises/05-input-output/5-18/README.md)를 따른다.

## 10. 추가 실습

- ★ **기초:** 출력 지정과 값형, 입력 지정과 포인터형을 두 표로 다시 작성한다.
- ★★ **응용:** 16자 token을 `%15s`로 읽고 저장된 15문자와 stream에 남은 1문자의 의미를 설명한다.
- ★★★ **도전:** 5-12의 compile-only mismatch 네 사례를 review 표에 합치고, 각 사례의 compiler diagnostic과 C17 의미를 분리한다.

## 11. 확인 문제

1. `printf("%f", float_value)`와 `scanf("%f", &float_value)`가 모두 `%f`를 쓰지만 내부 인자 계약은 어떻게 다른가?
2. `scanf("%d %lf", &count, &average)`가 1을 반환했다. 어떤 객체까지 새 값이 저장되었고 무엇을 아직 가정할 수 없는가?
3. 첫 변환 전에 입력 끝이 왔다. 반환값을 특정 숫자로 외우지 않고 어떻게 판정해야 하는가?
4. `char word[16]`에 `%16s`를 사용한 코드를 찾아 수정하고 이유를 설명하라.
5. `printf("%d\n", 3.14);`를 실행해 결과를 확인하면 안 되는 이유를 compiler diagnostic과 C17 의미로 나누어 설명하라.
6. `17 / 5`, `17 % 5`, `17.0 / 5.0`의 결과를 예측하고 각 피연산자형이 결과에 미친 영향을 설명하라.
7. `%p` 출력과 `%zu` 출력에서 표준이 보장하는 것과 현재 구현에서만 관찰하는 것을 각각 구별하라.

## 12. 핵심 정리

Part 5의 한 줄기는 **형식 문자열과 실제 인자형의 계약**, 다른 한 줄기는 **입력 상태와 계산 결과의 검증**이다. `printf`에는 값을, `scanf`에는 정확한 저장 포인터를 전달한다. 입력 성공 여부는 반환값으로 확인하고, 문자열 폭은 배열 용량보다 한 칸 작게 제한한다. 잘못된 format/type 대응은 실행 실험이 아니라 compile-only diagnostic과 C17 계약으로 분석한다. 계산은 피연산자형·단위·출력 서식을 함께 확인한다.

## 13. 다음 Step

Step 6-1. implicit conversion

## 14. 참고 자료

- [WG14 N2176, C17 ballot draft](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5, 6.5.2.2, 7.19, 7.21.6.1~2.
- [cppreference: C input/output](https://en.cppreference.com/w/c/io.html)
- [cppreference: `printf`](https://en.cppreference.com/w/c/io/fprintf.html)
- [cppreference: `scanf`](https://en.cppreference.com/w/c/io/fscanf.html)
- [GCC: warning options for format checking](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html#index-Wformat)
