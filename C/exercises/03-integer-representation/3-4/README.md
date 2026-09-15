# Step 3-4 실습: C 정수 리터럴
이론: [3-4](../../../notes/03-integer-representation/3-4-integer-literals.md)
## 준비
```sh
cd exercises/03-integer-representation/3-4
```
완성 답안은 제공하지 않는다.
## 필수 과제
- **목적:** 진법 표기가 달라도 같은 값을 나타냄을 확인한다.
- **작성할 파일:** `integer_literals.c`.
- **해야 할 일:** `42`, `052`, `0x2A`를 별도 `int`에 저장해 `%d`로 출력하고, `42U`를 `%o`, `%X`로 출력한다.
- **사용할 개념:** 접두사, suffix, `%d`, `%o`, `%X`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic integer_literals.c -o integer_literals
```
## 실행 방법
```sh
./integer_literals
```
## 예상 관찰 결과
세 `int`는 42이며 unsigned 값은 8진 52, 16진 2A로 보인다.
## 확인 포인트
- `08`, `0b`를 사용하지 않았는가?
- 출력 서식과 인수형이 맞는가?
- suffix를 폭 보장으로 설명하지 않았는가?
## 기록할 내용
| 항목 | 기록 |
|---|---|
| 빌드·실행 | |
| 세 표기와 값 | |
| 후보형 규칙 | |
## 추가 실습
- ★ `010` 비교
- ★★ suffix 후보표
- ★★★ 한계 매크로와 큰 상수 검토
## 완료 기준
- [ ] 직접 작성·빌드·실행했다.
- [ ] 세 진법과 후보형을 구별했다.
- [ ] C17에 표준 `0b`가 없음을 설명했다.
다음: [3-5](../../../notes/03-integer-representation/3-5-n-bit-unsigned-representation.md)
