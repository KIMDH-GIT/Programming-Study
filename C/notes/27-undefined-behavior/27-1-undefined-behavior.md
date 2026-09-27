# 27-1. Undefined Behavior
## 1. 학습 목표
- C17의 Undefined Behavior(UB)를 표준 용어로 정의한다.
- defined·undefined·unspecified·implementation-defined의 네 behavior 범주를 구분한다.
- constraint violation의 diagnostic 의무를 behavior 분류와 별도 축으로 판단한다.
- UB를 compiler 경고, crash, OS signal과 동일시하지 않는다.
## 2. 선수 지식
Part 7의 식 평가, Part 11의 배열 경계, Part 14의 유효한 역참조 대상을 안다.
## 3. 핵심 개념
**[C17 — Undefined Behavior]** 잘못된 프로그램 구성이나 잘못된 데이터를 사용하여, 표준이 해당 동작에 아무 요구사항도 부과하지 않는 경우다.

| behavior 범주 | C17의 요구 |
|---|---|
| defined behavior | 적용되는 규칙이 결과를 정한다 |
| undefined behavior | 표준이 요구사항을 부과하지 않는다 |
| unspecified behavior | 둘 이상의 허용 가능성 중 무엇을 택할지 정하지 않는다 |
| implementation-defined behavior | 구현이 선택하고 그 선택을 문서화한다 |

constraint violation은 다섯 번째 behavior 범주가 아니라 diagnostic 요구를 판단하는 별도 축이다. 번역 단위에 syntax rule 또는 constraint violation이 있으면 적어도 하나의 diagnostic이 요구된다. syntax error, compile error, linker의 `undefined reference`는 실행 중 UB와 같은 범주가 아니다. 제약 위반과 UB가 겹치는 경우도 있지만, diagnostic이 요구되지 않는 UB도 많다.
## 4. 문법
UB 전용 문법은 없다. 연산자와 library function의 조건을 위반할 때 발생한다.

⚠ 분석용 — 실행하지 않는다.
```c
int values[2] = {10, 20};
int bad = values[2]; /* array 범위 밖 access: UB */
```
## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int values[2] = {10, 20};
    size_t index = 1;

    if (index < sizeof values / sizeof values[0]) {
        printf("%d\n", values[index]);
    }
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o ub_intro
./ub_intro
```
## 6. 코드 해석
index를 실제 원소 수와 비교한 뒤 접근하므로 출력은 `20`이다. 이 정상 예제만 compile·run 대상으로 삼는다.
## 7. 내부 동작
**[C17 semantic category]** UB가 발생한 execution에는 표준이 결과를 정하지 않는다.

**[compiler diagnostic]** compiler acceptance나 warning 부재는 UB-free 증명이 아니다.

**[optimizer behavior]** compiler는 정의된 C 프로그램이라는 전제로 변환할 수 있다. UB를 발견해 일부러 망가뜨린다는 뜻은 아니다.

**[runtime / OS observation]** crash, 정상 종료, SIGSEGV는 가능한 관찰일 뿐 UB의 정의가 아니다.

**[sanitizer observation]** instrumentation이 일부 UB를 보고할 수 있으나 조용한 실행도 안전성 증명이 아니다.

**[CPU / ISA]** hardware가 특정 값을 만들거나 trap해도 그것이 C17의 보장이 되지는 않는다.
## 8. 자주 하는 실수
- “UB는 랜덤 값을 반환한다” 또는 “항상 crash한다”고 정의한다.
- strict warning 통과나 정상 출력을 안전성 증명으로 사용한다.
- implementation-defined와 unspecified behavior를 UB라고 부른다.
- `undefined reference`를 Undefined Behavior라고 해석한다.
## 9. 필수 실습
여러 사례를 네 behavior 범주로 분류하고 constraint violation 여부를 별도 표시한 뒤, 범위 검사된 정상 예제만 실행한다.
[27-1 exercise](../../exercises/27-undefined-behavior/27-1/README.md)
## 10. 추가 실습
- ★ defined와 UB 사례를 한 쌍으로 정리한다.
- ★★ diagnostic requirement 열을 표에 추가한다.
- ★★★ C17·compiler·OS·CPU 관찰을 네 열로 분리한다.
## 11. 확인 문제
1. C17에서 UB의 핵심 정의는 무엇인가?
2. warning이 없다는 사실로 UB가 없다고 결론 내릴 수 있는가?
3. unspecified behavior와 UB의 차이는?
4. segmentation fault가 UB의 정의가 아닌 이유는?
5. `undefined reference`와 UB는 왜 다른가?
## 12. 핵심 정리
- UB는 표준이 해당 동작에 요구사항을 부과하지 않는 범주다.
- compile 성공·warning 부재·정상 종료는 UB-free 증명이 아니다.
- 언어 의미와 compiler·sanitizer·OS·ISA 관찰을 분리한다.
## 13. 다음 Step
[27-2. implementation-defined behavior](27-2-implementation-defined-behavior.md)
## 14. 참고 자료
- N1570 3.4.1, 3.4.3, 3.4.4, 4, 5.1.1.3, Annex J. N1570은 **C11 공개 Committee Draft**이며 이 분류 규칙은 C17에서도 유지된다.
- [cppreference: Undefined behavior](https://en.cppreference.com/w/c/language/behavior)
