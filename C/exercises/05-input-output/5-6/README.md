# 5-6 실습: `sizeof` 출력

이론: [5-6 note](../../../notes/05-input-output/5-6-sizeof-output.md)

## 실습 목적
`size_t`, `%zu`, C byte를 연결한다.

## 작성할 파일
`sizeof_output.c`

## 해야 할 일

1. `<limits.h>`와 `<stdio.h>`를 포함한다.
2. `sizeof(char)`, `sizeof(int)`, `sizeof(double)`을 `%zu`로 출력한다.
3. `double value = 1.0;`을 선언하고 `sizeof value`를 `%zu`로 출력한다.
4. `CHAR_BIT`를 `%d`로 출력한다.
5. `sizeof(int) * CHAR_BIT`를 계산해 `int` 객체 표현이 차지하는 bit 수를 출력한다.
6. C byte 수와 bit 수를 별도 항목으로 기록하고 두 단위를 혼동하지 않는다.

## 사용할 개념
`sizeof`, `size_t`, `%zu`, C byte, `CHAR_BIT`.

## 컴파일 방법

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror sizeof_output.c -o sizeof_output
```

## 실행 방법

```sh
./sizeof_output
```

## 예상 관찰 결과

- `sizeof(char)`는 반드시 1이다.
- `sizeof(int)`, `sizeof(double)`, `sizeof value`는 양의 C byte 수로 표시된다.
- `sizeof value`와 `sizeof(double)`은 같다.
- `int`의 저장 bit 수는 출력된 `sizeof(int)`와 `CHAR_BIT`의 곱과 같다.
- `sizeof(int)`와 `CHAR_BIT`의 구체적인 값은 현재 구현의 관찰값이므로 한 숫자로 고정하지 않는다.

## 확인 포인트

- `%zu`를 사용했는가?
- `sizeof` 결과형이 `size_t`임을 설명했는가?
- `sizeof(char) == 1`과 `CHAR_BIT == 8`을 같은 보장으로 취급하지 않았는가?
- byte와 bit 계산의 관계를 실제 출력값으로 확인했는가?

## 추가 실습

- ★ **기초:** `short`, `long`, `long double`의 C byte 크기를 추가한다.
- ★★ **응용:** 각 기본형의 `sizeof(type) * CHAR_BIT`를 표로 정리한다.
- ★★★ **도전:** 구현 차이를 기록한다.

## 완료 기준

- [ ] strict C17 옵션과 `-Werror`에서 진단 없이 compile된다.
- [ ] `char`, `int`, `double`, `value`, `CHAR_BIT`, `int` bit 수를 모두 출력했다.
- [ ] `sizeof(char)`가 1임을 확인했다.
- [ ] `sizeof value == sizeof(double)` 관계를 확인했다.
- [ ] `sizeof(int) * CHAR_BIT` 계산과 출력값이 일치한다.
- [ ] C byte와 bit 단위를 정확히 기록했다.
- [ ] 구현 관찰값을 C17 전체의 고정값으로 일반화하지 않았다.
- [ ] 답안 `.c`를 제공하지 않았다.
