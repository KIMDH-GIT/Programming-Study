# 5-3 실습: 부동소수점 출력 정밀도

이론: [5-3 note](../../../notes/05-input-output/5-3-floating-output-precision.md)

## 실습 목적
`%f`, `%Lf`, 정밀도 표기를 구별한다.

## 작성할 파일
`floating_output.c`

## 해야 할 일

1. `float small = 1.25F;`, `double normal = 3.5;`, `long double wide = 2.75L;`을 선언한다.
2. `small`을 `%.2f`와 `%.4f`로 출력한다.
3. `normal`을 `%.2f`와 `%.4f`로 출력한다.
4. `wide`를 `%.2Lf`와 `%.4Lf`로 출력한다.
5. 각 객체를 출력한 뒤 원래 객체의 형과 값이 바뀌지 않았음을 설명한다.

## 사용할 개념
기본 인수 승격, `%f`, `%Lf`, `%.nf`.

## 컴파일 방법

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror floating_output.c -o floating_output
```

## 실행 방법

```sh
./floating_output
```

## 예상 관찰 결과

```text
float short=1.25
float long=1.2500
double short=3.50
double long=3.5000
long double short=2.75
long double long=2.7500
```

정밀도 4는 표시할 소수 자릿수를 늘려 뒤에 0을 추가하지만 객체의 저장형과 저장값을 바꾸지 않는다.

## 확인 포인트

- `float` 인자가 기본 인수 승격 뒤 `%f`와 맞는 이유를 설명할 수 있는가?
- `long double`에 `%Lf`를 썼는가?
- `%.2f`와 `%.4f`의 숫자가 저장 정밀도가 아니라 표시 자릿수임을 구별했는가?
- 여섯 출력 줄이 고정 fixture의 예상 결과와 일치하는가?

## 추가 실습

- ★ **기초:** `normal`을 정밀도 1과 6으로 출력하고 추가되는 표시 자릿수를 기록한다.
- ★★ **응용:** `0.1`을 여러 자릿수로 출력하고 이진 부동소수점 근사를 관찰한다.
- ★★★ **도전:** 구현의 `long double` 특성을 기록한다.

## 완료 기준

- [ ] strict C17 옵션과 `-Werror`에서 진단 없이 compile된다.
- [ ] 여섯 출력 줄이 예상 결과와 일치한다.
- [ ] `float`/`double` 출력과 `long double` 출력의 서식을 구별했다.
- [ ] 출력 정밀도의 의미를 설명했다.
- [ ] 출력 전후 객체의 형과 저장값이 바뀌지 않음을 설명했다.
- [ ] 답안 소스를 제공하지 않았다.
