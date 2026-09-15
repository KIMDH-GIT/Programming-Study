# Step 2-6 실습: `sizeof`, `size_t`, byte와 `CHAR_BIT`

이론: [2-6. `sizeof`, `size_t`, byte와 `CHAR_BIT`](../../../notes/02-variables-and-types/2-6-sizeof-size-t-byte-char-bit.md)

완성 답안 소스는 제공하지 않는다. 모든 C 프로그램은 학습자가 직접 작성한다. 기본형 변수와 출력만 사용하며 배열, 포인터, 조건문, 반복문, 동적 메모리는 필요하지 않다. 선택 도전의 비트 수 계산은 종이나 계산기로 수행해도 된다.

## 준비

- C17 호스트 환경과 **[GCC/Linux 구현]** 셸을 사용한다.
- 저장소의 `C/` 디렉터리를 기준으로 작업 디렉터리로 이동한다.
- `<stddef.h>`는 `size_t`, `<stdio.h>`는 `printf`, `<limits.h>`는 `CHAR_BIT`를 위해 포함한다.
- `sizeof`는 함수가 아니라 연산자다. 형 이름에는 괄호를 사용한다.

```sh
cd exercises/02-variables-and-types/2-6
gcc --version
```

## 필수 실습 A — 기초: 형 크기와 단위 기록

- **목적:** `sizeof`의 결과형과 단위, 한 C byte의 비트 수를 직접 확인한다.
- **해야 할 일:** `type_sizes.c`에 `int main(void)`를 작성한다. `char`, `short`, `int`, `long`, `long long`, `float`, `double`, `long double` 각각의 크기를 한 줄씩 출력한다. `CHAR_BIT`도 별도 줄로 출력한다. 초기화한 `int` 변수 하나의 크기를 `size_t` 변수에 저장하고 별도 줄로 출력한다. 총 열 줄을 만들며 마지막은 `return 0;`으로 끝낸다.
- **사용할 개념:** `sizeof(형)`, `sizeof 변수`, `size_t`, `%zu`, `%d`, `CHAR_BIT`, 선언·초기화.
- **예상 관찰 결과:** `sizeof(char)`는 1, `CHAR_BIT`는 8 이상이다. `int` 변수의 크기는 `sizeof(int)`와 같다. 다른 크기는 구현에 따라 달라지므로 고정 숫자로 채점하지 않는다.
- **확인 포인트:** 크기를 모두 `%zu`로, `CHAR_BIT`를 `(int)`로 변환한 뒤 `%d`로 출력했는가? 각 크기에 `C bytes`, `CHAR_BIT`에 `bits per C byte`처럼 단위를 적었는가? 크기 숫자를 직접 적지 않고 `sizeof`로 구했는가?

출력 줄은 `sizeof(int) = 측정값 C bytes`처럼 이름·값·단위가 구분되게 한다. 한 C byte의 비트 수가 8이 아닌 구현도 허용된다. 정수형 크기는 `char <= short <= int <= long <= long long` 순서이며 인접한 크기가 같아도 정상이다.

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic type_sizes.c -o type_sizes && ./type_sizes
printf 'status=%s\n' "$?"
```

`&&`는 성공한 빌드 뒤에만 실행한다. 바로 다음의 `$?`는 실행했다면 실행 상태, 빌드가 실패했다면 빌드 상태다. 실패 뒤 예전 실행 파일을 별도로 실행하지 않는다.

## 추가 실습 B — 응용: 값은 달라도 크기는 같은가

- **목적:** 값의 십진 자릿수와 객체의 저장 크기를 구별한다.
- **해야 할 일:** `value_and_size.c`에 두 `int` 변수를 선언하고 각각 `7`, `700`으로 초기화한다. 각 변수에 대해 값과 크기를 한 줄에 출력한다. 값에는 `%d`, 크기에는 `%zu`를 사용한다. 두 줄을 읽고 크기 관계와 이유를 기록한다.
- **사용할 개념:** 변수 초기화, 서식과 인수형 대응, `sizeof`와 형 정보.
- **예상 관찰 결과:** 값은 서로 다르지만 크기는 같다. 크기가 반드시 4 C bytes여야 하는 것은 아니다.
- **확인 포인트:** 출력된 숫자의 글자 수와 C byte 수를 혼동하지 않았는가? 같은 `int`형이므로 크기가 같다고 설명했는가?

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic value_and_size.c -o value_and_size && ./value_and_size
printf 'status=%s\n' "$?"
```

## 추가 실습 C — 선택 도전: byte에서 bit로 해석

- **목적:** C byte, bit, 값 표현 능력을 구별한다.
- **해야 할 일:** 필수 실습에서 얻은 `char`, `int`, `double`의 C byte 수와 `CHAR_BIT`를 표에 옮긴다. 각 행의 저장 비트 수를 수학적으로 곱해 계산한다. 이어 `CHAR_BIT`가 16이고 `sizeof(int)`가 2인 가상 구현의 `int`도 계산한다. 추가 C 소스나 산술 연산자 코드는 요구하지 않는다.
- **사용할 개념:** C byte 수 × byte당 비트 수, octet, 객체 표현과 패딩.
- **예상 관찰 결과:** 가상 구현의 `int` 저장 공간은 32비트다. 그 구현에서도 `sizeof(char)`는 1이며 `char` 저장 공간은 16비트다.
- **확인 포인트:** 한 C byte를 무조건 8비트로 바꾸지 않았는가? 총 저장 비트 수만으로 정수 최댓값이나 `double`의 유효 자릿수를 단정하지 않았는가?

| 대상 | C byte 수 | `CHAR_BIT` | 총 저장 비트 수 |
|---|---:|---:|---:|
| 내 구현의 `char` | | | |
| 내 구현의 `int` | | | |
| 내 구현의 `double` | | | |
| 가상 구현의 `int` | 2 | 16 | |

## 기록할 내용

| 항목 | 직접 기록 |
|---|---|
| GCC 버전과 대상 환경 | |
| 작성한 소스와 실행 명령 | |
| 빌드 경고 유무와 종료 상태 | |
| 필수 실습의 열 줄 출력 | |
| `size_t`에 `%zu`를 쓰는 이유 | |
| C17이 보장하는 관계 | |
| 내 구현에서만 측정한 구체적인 크기 | |
| 선택한 추가 과제 결과 | |

## 완료 기준

- [ ] 필수 파일 `type_sizes.c`를 직접 작성하고 경고 없이 빌드·실행했다.
- [ ] 여덟 기본 자료형의 크기, `CHAR_BIT`, `int` 변수의 크기를 기록했다.
- [ ] `size_t` 변수를 실제로 사용하고 `sizeof` 결과를 `%zu`로 출력했다.
- [ ] `sizeof(char) == 1`을 한 C byte라고 설명하며 `CHAR_BIT`와 구분했다.
- [ ] 특정 형의 크기, 한 byte의 8비트 여부를 모든 C 구현의 사실로 일반화하지 않았다.
- [ ] 선택 과제의 수행 여부를 명시했으며 기록에 실제 관찰과 가상 예시를 구분했다.

다음 제목: [2-7. `<limits.h>`와 정수 범위](../../../C_CURRICULUM.md#part-2-변수와-자료형). 이 링크는 교육과정의 Part 2 목록으로 연결된다. 표준 근거는 [이론의 참고 자료](../../../notes/02-variables-and-types/2-6-sizeof-size-t-byte-char-bit.md#14-참고-자료)에서 확인한다.
