# Step 3-8 실습: unsigned 순환과 signed overflow

이론: [3-8](../../../notes/03-integer-representation/3-8-unsigned-wrap-signed-overflow.md)

완성 답안은 제공하지 않는다. signed overflow 식은 실행하지 않는다.

## 준비
```sh
cd exercises/03-integer-representation/3-8
```

## 필수 과제
- **실습 목적:** unsigned 경계 관계를 관찰하고 signed UB와 구별한다.
- **작성할 파일:** `unsigned_boundaries.c`.
- **해야 할 일:** `UINT_MAX`, `UINT_MAX+1U`, `0U-1U`를 별도 `unsigned int` 변수로 출력한다. `INT_MAX+1`, `INT_MIN-1`, -1의 unsigned 변환을 실행 없이 분류한다.
- **사용할 개념:** modulo, signed overflow UB, 정수 변환, `%u`.

## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic unsigned_boundaries.c -o unsigned_boundaries
```
## 실행 방법
```sh
./unsigned_boundaries
```

## 예상 관찰 결과
둘째 값은 0이고 셋째 값은 첫째 값과 같다. 구체적 최댓값은 구현 의존이다.

## 확인 포인트
- signed overflow를 코드에 넣지 않았는가?
- 세 변수와 `%u`가 일치하는가?
- 표준 관계와 구현 숫자를 구별했는가?
- 정의된 결과와 업무상 안전을 구별했는가?

## 기록할 내용
| 항목 | 기록 |
|---|---|
| 빌드·실행 | |
| 출력 관계 | |
| UB 분류 | |
| 표준/구현 구분 | |

## 추가 실습
- ★ 수학적 modulo 계산
- ★★ unsigned long 경계
- ★★★ `-fwrapv` 문서 비교

## 완료 기준
- [ ] 직접 작성해 경고 없이 빌드했다.
- [ ] unsigned 두 관계를 확인했다.
- [ ] signed overflow를 실행하지 않았다.
- [ ] 산술과 변환을 구별했다.

다음: [3-9](../../../notes/03-integer-representation/3-9-part-3-review.md)
