# Step 3-7 실습: 같은 비트 패턴의 signed·unsigned 해석

이론: [3-7](../../../notes/03-integer-representation/3-7-same-bits-signed-unsigned.md)

완성 답안은 제공하지 않는다. 객체 표현을 조사하지 않고 값 변환과 종이 모형을 별도로 기록한다.

## 준비

```sh
cd exercises/03-integer-representation/3-7
```

## 필수 과제

- **실습 목적:** 공통 범위와 음수의 unsigned 변환을 안전하게 관찰한다.
- **작성할 파일:** `integer_conversion.c`.
- **해야 할 일:** 42를 `int`→`unsigned int`→`int`로 변환하고, -1을 `unsigned int`로 변환하여 `UINT_MAX`와 함께 출력한다. `00000101`, `10000101`, `11111111`을 가상의 padding 없는 8-bit 네 모형으로 계산한다.
- **사용할 개념:** 값 변환, 공통 범위, unsigned modulo 변환, `%d`, `%u`.

## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic integer_conversion.c -o integer_conversion
```
## 실행 방법
```sh
./integer_conversion
```

## 예상 관찰 결과

42는 왕복 후 유지되고 -1에서 변환된 값은 `UINT_MAX`와 같다. 종이표 결과는 선택한 표현에 따라 다르다.

## 확인 포인트

- cast를 bit 재해석이라고 하지 않았는가?
- 범위 밖 signed 변환과 signed overflow를 실행하지 않았는가?
- `%d`, `%u` 인수형이 맞는가?
- 종이표를 실제 객체 관찰이라고 하지 않았는가?

## 기록할 내용
| 항목 | 기록 |
|---|---|
| 빌드·실행 상태 | |
| 공통 범위 결과 | |
| -1 변환과 `UINT_MAX` | |
| 가상 패턴표 | |
| 표준/구현 구분 | |

## 추가 실습
- ★ 공통 범위 값 세 개 비교
- ★★ 네 표현 모형 확장
- ★★★ 구현 문서 조사

## 완료 기준
- [ ] 직접 작성해 경고 없이 빌드했다.
- [ ] 값 변환 관계를 확인했다.
- [ ] 가상 패턴과 실제 객체를 구별했다.
- [ ] 이후 Part 문법을 사용하지 않았다.

다음: [3-8](../../../notes/03-integer-representation/3-8-unsigned-wrap-signed-overflow.md)
