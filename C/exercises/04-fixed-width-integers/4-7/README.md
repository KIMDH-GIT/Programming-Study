# 4-7 실습: Part 4 종합 복습

이론: [4-7. Part 4 종합 복습](../../../notes/04-fixed-width-integers/4-7-part-4-review.md)

## 실습 목적

일반 정수형과 세 폭 기반 계열을 목적에 맞게 선택하고, exact-width 값의 한계·상수·출력 매크로를 통합한다.

## 작성할 파일

`part4_review.c`와 형 선택 기록표를 작성한다.

## 해야 할 일

1. 일반 계산용 `int` 변수를 만든다.
2. 같은 최소 범위의 `int_least16_t`, `int_fast16_t` 변수를 만든다.
3. `int32_t` 음수와 `uint32_t` 레지스터 값 모델을 만든다.
4. 상수에 `INTN_C`, `UINTN_C`를 사용한다.
5. exact-width 값과 한계를 대응 `PRI` 매크로로 출력한다.
6. `sizeof(uint32_t)`와 `CHAR_BIT`를 출력하고 단위를 기록한다.
7. 각 변수의 선택 이유와 표준/구현 구분을 표로 정리한다.

## 사용할 개념

일반 `int`, exact-width, minimum-width, fastest minimum-width, integer limit macro, integer constant macro, `PRI` 출력 매크로, C byte와 bit.

## 컴파일 방법

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic part4_review.c -o part4_review
```

## 실행 방법

```sh
./part4_review
```

## 예상 관찰 결과

모든 값이 대응 서식으로 출력된다. `uint32_t`의 `sizeof`는 C byte 수이며, least와 fast 형의 실제 크기는 구현 선택으로 관찰한다.

## 확인 포인트

- 정확한 외부 폭이 없는 값에 `int`를 사용했는가?
- least와 fast를 정확한 폭이라고 쓰지 않았는가?
- exact-width 형의 선택 제공을 기록했는가?
- 모든 `PRI` 매크로 앞에 필요한 `%`를 붙였는가?
- 레지스터 값 모델과 실제 하드웨어 접근을 구별했는가?
- Part 5 이후 문법을 불필요하게 쓰지 않았는가?

## 추가 실습

- ★ **기초:** 형 선택 사례 다섯 개를 표로 분류한다.
- ★★ **응용:** 8-bit least와 fast 형의 크기를 비교한다.
- ★★★ **도전:** 통신 필드에서 exact-width 선택 뒤에도 남는 byte 순서와 검증 문제를 정리한다.

## 완료 기준

- [ ] `part4_review.c`가 지정한 C17 경고 옵션으로 진단 없이 컴파일된다.
- [ ] 실행 상태 0이다.
- [ ] 다섯 변수의 선택 이유를 설명했다.
- [ ] exact, least, fast의 계약을 구별했다.
- [ ] 한계·상수·출력 매크로를 올바르게 사용했다.
- [ ] C byte와 bit, 표준과 구현을 구별했다.
- [ ] 실제 하드웨어 접근을 구현하지 않았다.
- [ ] 완성 답안 소스를 README에 포함하지 않았다.

## 다음 Step

Step 5-1. `printf` 서식 문자열과 인자 대응
