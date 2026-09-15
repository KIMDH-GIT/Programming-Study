# Step 3-5 실습: N-bit unsigned 표현
이론: [3-5](../../../notes/03-integer-representation/3-5-n-bit-unsigned-representation.md)
## 준비
```sh
cd exercises/03-integer-representation/3-5
```
완성 답안은 제공하지 않는다.
## 필수 과제
- **목적:** 수학적 N-bit 모형과 C 구현 한계를 연결한다.
- **작성할 파일:** `unsigned_range.c`.
- **해야 할 일:** `CHAR_BIT`, `UCHAR_MAX`, `UINT_MAX`, `UINT_MAX+1U`를 출력하고 N=4, 8의 범위를 계산한다.
- **사용할 개념:** value bit, padding, modulo, 한계 매크로.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic unsigned_range.c -o unsigned_range
```
## 실행 방법
```sh
./unsigned_range
```
## 예상 관찰 결과
마지막 결과는 0이고 구체적 한계는 구현에 따라 다를 수 있다.
## 확인 포인트
- `UCHAR_MAX` cast와 출력형이 맞는가?
- 저장 bit와 값 bit를 구별했는가?
- N=8 예를 모든 형에 일반화하지 않았는가?
## 기록할 내용
| 항목 | 기록 |
|---|---|
| 환경·상태 | |
| 한계 출력 | |
| N=4,8 계산 | |
## 추가 실습
- ★ N=3 표
- ★★ modulo 관계
- ★★★ padding 조사
## 완료 기준
- [ ] 직접 작성해 경고 없이 빌드했다.
- [ ] 두 범위를 계산했다.
- [ ] 표준과 구현을 구별했다.
다음: [3-6](../../../notes/03-integer-representation/3-6-twos-complement-c17-signed-representations.md)
