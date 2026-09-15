# Step 3-1 실습: bit·byte와 자릿값

이론: [3-1. bit·byte와 자릿값](../../../notes/03-integer-representation/3-1-bit-byte-place-value.md)

완성 답안 소스는 제공하지 않는다. 학습자가 `bit_byte.c`를 직접 작성하고 계산·관찰 결과를 기록한다.

## 준비

- C17 호스트 환경과 GCC가 설치된 Linux 셸을 사용한다.
- 저장소의 `C/`에서 이 디렉터리로 이동한다.
- `<limits.h>`, `<stdio.h>`, `sizeof`, `%zu`, `%d`를 복습한다.
- 조건문, 반복문, 배열, 포인터, 비트 연산자, shift는 사용하지 않는다.

```sh
cd exercises/03-integer-representation/3-1
gcc --version
```

## 필수 과제: 저장 단위와 이진 자릿값

- **목적:** C byte와 bit의 단위를 구별하고 이진 자릿값으로 값을 계산한다.
- **작성할 파일:** 이 디렉터리의 `bit_byte.c`.
- **사용할 개념:** bit, C byte, octet, `sizeof`, `size_t`, `CHAR_BIT`, `%zu`, `%d`.

### 해야 할 일

1. `<limits.h>`와 `<stdio.h>`를 포함하고 `int main(void)`를 작성한다.
2. `sizeof(char)`를 `%zu`로 출력하고 단위를 `C byte`라고 적는다.
3. `CHAR_BIT`를 `(int)`로 변환하여 `%d`로 출력하고 `bits per C byte`라고 적는다.
4. `sizeof(unsigned int)`를 `%zu`로 출력한다.
5. `return 0;`으로 끝낸다.
6. 종이에 `1011₂`를 `1×2³ + 0×2² + 1×2¹ + 1×2⁰`으로 풀고 십진 값을 적는다.
7. 관찰한 `CHAR_BIT`가 표준 보장인지 현재 구현의 구체적 값인지 구분한다.

## 컴파일

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic bit_byte.c -o bit_byte
printf 'build status=%s\n' "$?"
```

## 실행

빌드 상태가 0일 때만 실행한다.

```sh
./bit_byte
printf 'run status=%s\n' "$?"
```

또는 성공한 빌드만 실행하도록 연결한다.

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic bit_byte.c -o bit_byte && ./bit_byte
```

## 예상 관찰 결과

- `sizeof(char)`는 1이다.
- `CHAR_BIT`는 8 이상이다. 흔한 환경에서는 8이지만 고정 정답이 아니다.
- `sizeof(unsigned int)`의 값은 구현에 따라 다를 수 있다.
- `1011₂`의 십진 값은 11이다.

## 확인 포인트

- 크기에 `%zu`, 변환한 `CHAR_BIT`에 `%d`를 사용했는가?
- `sizeof(char)`의 1을 1 bit라고 쓰지 않았는가?
- 현재 구현의 `CHAR_BIT` 값과 C17의 최소 보장을 구별했는가?
- 전체 저장 bit 수를 값 bit 수라고 단정하지 않았는가?
- 이진수 계산을 실제 메모리 관찰이라고 부르지 않았는가?

## 기록할 내용

| 항목 | 직접 기록 |
|---|---|
| GCC 버전과 대상 | |
| 전체 빌드 명령과 상태 | |
| 실행 출력과 상태 | |
| `sizeof(char)` 값과 단위 | |
| `CHAR_BIT` 값과 단위 | |
| `sizeof(unsigned int)` 값 | |
| `1011₂`의 자릿값 계산 | |
| C17 보장 한 가지 | |
| 현재 구현 관찰 한 가지 | |

## 추가 과제

- ★ **기초:** `1101₂`를 십진수로 바꾸고 자릿값 식을 쓴다.
- ★★ **응용:** `sizeof(unsigned int) * CHAR_BIT`를 종이에서 계산해 전체 저장 bit 수로 기록한다.
- ★★★ **도전:** `CHAR_BIT=16`, `sizeof(unsigned int)=2`인 가상 구현을 계산하고 값 bit 수가 자동으로 확정되지 않는 이유를 쓴다.

## 완료 기준

- [ ] `bit_byte.c`를 직접 작성하고 경고·오류 없이 빌드했다.
- [ ] 성공한 실행의 출력과 종료 상태를 기록했다.
- [ ] `sizeof(char) == 1`과 `CHAR_BIT >= 8`을 구별했다.
- [ ] `1011₂`를 자릿값으로 풀어 11을 얻었다.
- [ ] 표준 보장과 구현 관찰을 분리했다.
- [ ] 완성 답안 파일을 복사하지 않고 직접 작성했다.

다음 학습: [Step 3-2. 10진수와 2진수](../../../notes/03-integer-representation/3-2-decimal-binary.md).
