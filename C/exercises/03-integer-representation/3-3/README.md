# Step 3-3 실습: 16진수와 2진수
이론: [3-3](../../../notes/03-integer-representation/3-3-hexadecimal-binary.md)

## 준비
```sh
cd exercises/03-integer-representation/3-3
```
완성 답안은 제공하지 않는다.

## 필수 과제
- **목적:** hex digit과 네 이진 자리를 연결한다.
- **작성할 파일:** `hex_binary.c`.
- **해야 할 일:** `0xA5U`를 `%u`, `%X`로 출력하고 `A=1010`, `5=0101` 계산을 기록한다.
- **사용할 개념:** 16진 자릿값, `%u`, `%X`, `U`.

## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic hex_binary.c -o hex_binary
```
## 실행 방법
```sh
./hex_binary
```
## 예상 관찰 결과
십진 165와 16진 A5가 같은 값으로 표시된다.
## 확인 포인트
- `%X` 인수가 `unsigned int`인가?
- 표기와 실제 byte 배치를 구별했는가?
- 한 C byte를 무조건 8 bit라고 하지 않았는가?
## 기록할 내용
| 항목 | 기록 |
|---|---|
| 빌드·실행 상태 | |
| `0xA5` 변환 | |
| 표준/구현 구분 | |
## 추가 실습
- ★ `0x2D` 변환
- ★★ `1111 0000₂` 변환
- ★★★ 숫자 표기와 객체 표현의 차이 설명
## 완료 기준
- [ ] 직접 작성해 경고 없이 빌드했다.
- [ ] 16진 한 자리를 네 bit로 바꿨다.
- [ ] 숫자 표기와 메모리 배치를 구별했다.
다음: [3-4](../../../notes/03-integer-representation/3-4-integer-literals.md)
