# 25-5. `SQUARE(x)`와 괄호
## 1. 학습 목표
- macro arguments와 전체 replacement expression을 괄호로 보호한다.
- 잘못된 expansion의 expression structure를 분석한다.
- 정상 예제에는 side-effect-free argument만 사용한다.
## 2. 선수 지식
25-4 function-like macro와 Part 7 operator precedence를 안다.
## 3. 핵심 개념
나쁜 definition:
```c
#define SQUARE(x) x * x
```

`SQUARE(1 + 2)`는 `1 + 2 * 1 + 2` tokens로 확장되어 9가 아니라 5로 평가될 수 있다.

기본 권장 형태:
```c
#define SQUARE(x) ((x) * (x))
```
argument occurrences와 replacement expression 전체를 괄호로 감싼다.
## 4. 문법
```c
#define SQUARE(x) ((x) * (x))
#define DOUBLE(x) ((x) + (x))
```

모든 macro가 expression macro는 아니다. 이 괄호 규칙은 expression-like replacement를 안전하게 조합하기 위한 기본 pattern이다.
## 5. 최소 코드 예제
`main.c`
```c
#include <stdio.h>

#define SQUARE(x) ((x) * (x))

int main(void)
{
    const int result = SQUARE(1 + 2);

    printf("%d\n", result);
    return 0;
}
```

```sh
gcc -std=c17 -E main.c
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    main.c -o square_app
./square_app
```
## 6. 코드 해석
올바른 macro는 `((1 + 2) * (1 + 2))`와 같은 expression structure를 만든다. 괄호는 precedence와 surrounding expression 결합을 보호한다.
## 7. 내부 동작
**[C translation phases / preprocessing]** preprocessor는 parameter occurrences를 argument preprocessing tokens로 대체한다.

**[C compiler]** 확장된 expression의 operator precedence와 types를 적용한다.

**[GCC driver / option]** `-E` 출력에서 잘못된 definition과 올바른 definition의 token structure를 비교할 수 있다.

**[build system]** shared header의 macro 수정은 여러 translation units의 결과를 바꿀 수 있다.

**[OS / CPU]** 괄호 자체는 runtime function boundary를 만들지 않는다. compiler가 번역한 expression을 실행한다.
## 8. 자주 하는 실수
- argument만 괄호로 감싸고 replacement 전체 괄호를 생략한다.
- `SQUARE(x)`를 function처럼 argument 한 번 평가가 보장된다고 생각한다.
- 괄호를 넣으면 side-effect argument도 안전해진다고 생각한다.
- `SQUARE(i++)` 같은 위험 예제를 정상 실행 실습으로 사용한다.
- compiler warning이 없으면 macro가 안전하다고 판단한다.
## 9. 필수 실습
`SQUARE(1 + 2)`의 잘못된 expansion과 올바른 expansion을 종이에 전개하고 안전한 version만 실행한다.
[25-5 exercise](../../exercises/25-preprocessor/25-5/README.md)
## 10. 추가 실습
- ★ `SQUARE(2 + 3)`을 계산한다.
- ★★ `18 / SQUARE(3)`에서 replacement 전체 괄호가 있을 때 2, 없을 때 18이 되는 이유를 설명한다.
- ★★★ 같은 기능의 typed function이 가진 장점을 정리한다.
## 11. 확인 문제
1. `SQUARE(1 + 2)`가 나쁜 macro에서 어떻게 확장되는가?
2. argument occurrence마다 괄호가 필요한 이유는?
3. replacement 전체 괄호가 필요한 이유는?
4. 괄호가 multiple evaluation을 제거하는가?
5. 위험 macro example을 실행하지 않는 이유는?
## 12. 핵심 정리
- expression macro는 arguments와 replacement 전체를 괄호로 보호한다.
- expansion 결과의 operator structure를 직접 확인한다.
- 괄호는 multiple evaluation 문제를 해결하지 않는다.
## 13. 다음 Step
[25-6. macro 인자 중복 평가](25-6-macro-argument-multiple-evaluation.md)
## 14. 참고 자료
- N1570 6.5, 6.10.3.1. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [GCC: Operator Precedence Problems](https://gcc.gnu.org/onlinedocs/cpp/Operator-Precedence-Problems.html)
- [cppreference: Operator precedence](https://en.cppreference.com/w/c/language/operator_precedence)
