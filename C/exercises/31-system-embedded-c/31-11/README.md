# 31-11 실습: pointer casting·alignment·aliasing
이론: [note](../../../notes/31-system-embedded-c/31-11-pointer-casting-alignment-and-aliasing.md)
## 실습 목적
same-type `void *` round-trip과 character byte copy를 정의된 동작으로 실행한다.
## 작성할 파일
- `main.c`
## 해야 할 일
live `uint32_t` object를 `void *`로 전달해 같은 type pointer로 복원한다. object bytes는 `memcpy`로 `unsigned char` array에 복사한다. incompatible/misaligned cast dereference는 실행하지 않는다.
## 사용할 개념
pointer conversion, alignment, effective type, character access, `memcpy`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o pointer_rules
```
## 실행 방법
```sh
./pointer_rules
```
## 예상 관찰 결과
`value=12345687`, byte count는 `sizeof(uint32_t)`다. 출력한 첫 byte 값은 host-dependent observation으로만 기록한다.
## 확인 포인트
cast와 valid dereference를 구분한다. strict aliasing을 주소가 달라야 한다는 규칙으로 단순화하지 않는다.
## 추가 실습
- ★ `memcpy`로 same type object에 복원한다.
- ★★ alignment precondition을 서술한다.
- ★★★ invalid 예제를 실행하지 않고 위반 규칙만 분석한다.
## 완료 기준
정의된 access만 실행하고 host-dependent byte에는 exact 값을 강제하지 않는다.
