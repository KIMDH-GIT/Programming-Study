# 21-6 실습: C17 signed shift 규칙
이론: [note](../../../notes/21-bitwise-operators/21-6-c17-signed-shift-rules.md)
## 실습 목적
defined, UB, implementation-defined signed shifts를 구분한다.
## 작성할 파일
`signed_shift_rules.c`
## 해야 할 일
정의된 positive example만 실행하고 위험 expressions는 주석 표로 분류한다.
## 사용할 개념
signed shift, representability, UB, implementation-defined.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic signed_shift_rules.c -o signed_shift_rules
```
## 실행 방법
```sh
./signed_shift_rules
```
## 예상 관찰 결과
정의된 결과 12만 출력된다.
## 확인 포인트
negative·overflow·invalid-count cases를 실행하지 않는다.
## 추가 실습
- ★ 조건을 서술한다.
- ★★ unsigned로 바꾼다.
- ★★★ behavior 분류표를 만든다.
## 완료 기준
위험 cases 없이 C17 분류를 설명한다.
