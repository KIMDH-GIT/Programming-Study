# 25-6. macro 인자 중복 평가
## 1. 학습 목표
- macro argument가 replacement에 여러 번 나타나는 위험을 설명한다.
- multiple evaluation과 unsequenced undefined behavior를 구분한다.
- 위험 예제는 분석만 하고 실행하지 않는다.
## 2. 선수 지식
25-5 macro 괄호와 Part 7 evaluation order를 안다.
## 3. 핵심 개념
```c
#define SQUARE(x) ((x) * (x))
```

argument `x`는 replacement에 두 번 나타난다. `SQUARE(4)`는 안전하지만 `SQUARE(i++)`는 개념적으로:

```c
((i++) * (i++))
```

가 되어 같은 scalar object에 대한 두 modifications가 서로 unsequenced인 문제가 생기며 C17 undefined behavior다. “두 번 증가해서 값이 이상하다” 정도의 논리 오류가 아니다.
## 4. 문법
```c
#define MIN(a, b) ((a) < (b) ? (a) : (b))
```

`MIN`도 condition에서 사용한 selected argument를 다시 평가할 수 있다. multiple evaluation이 항상 UB인 것은 아니지만 function call·I/O·increment 같은 side effects가 예상보다 여러 번 일어날 수 있으므로 안전한 generic function처럼 사용하지 않는다.
## 5. 최소 코드 예제
`main.c`
```c
#include <stdio.h>

#define SQUARE(x) ((x) * (x))

int main(void)
{
    const int value = 4;
    const int result = SQUARE(value);

    printf("%d\n", result);
    return 0;
}
```

```sh
gcc -std=c17 -E main.c
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    main.c -o safe_macro_app
./safe_macro_app
```
## 6. 코드 해석
normal example은 side-effect-free expression만 전달한다. `SQUARE(i++)`는 source 분석 문제로만 다루며 compile 성공이나 warning 부재를 안전성 증거로 삼지 않는다.
## 7. 내부 동작
**[C translation phases / preprocessing]** macro argument tokens가 replacement의 각 parameter occurrence에 들어간다.

**[C compiler]** 확장된 expression에 C17 sequencing rules를 적용한다. 위험은 preprocessor가 아니라 expansion 결과의 C expression semantics에서 드러난다.

**[GCC driver / option]** `-E`는 duplicate occurrences를 보여줄 수 있지만 UB를 판정하는 완전한 안전 분석기가 아니다.

**[build system]** compiler flags와 warnings는 유용하지만 multiple evaluation의 모든 위험을 진단한다고 보장할 수 없다.

**[OS / CPU]** UB program의 실행 결과를 관찰해 의미를 정의하려 해서는 안 된다.
## 8. 자주 하는 실수
- 괄호가 multiple evaluation을 막는다고 생각한다.
- `SQUARE(i++)`를 실행해 결과를 예측한다.
- 모든 multiple evaluation을 무조건 UB라고 부른다.
- `MIN(read_value(), limit)`이 function처럼 argument를 한 번만 평가한다고 생각한다.
- warning 없는 build를 macro safety 증거로 삼는다.
## 9. 필수 실습
`SQUARE(value)`만 실행하고 `SQUARE(i++)` expansion은 종이에 전개해 UB 원인을 설명한다.
[25-6 exercise](../../exercises/25-preprocessor/25-6/README.md)
## 10. 추가 실습
- ★ `SQUARE(5)`를 안전하게 계산한다.
- ★★ `MIN(a, b)`에서 어느 argument가 두 번 평가될 수 있는지 표시한다.
- ★★★ 같은 기능을 typed function으로 바꿨을 때 평가 횟수를 비교한다.
## 11. 확인 문제
1. macro argument가 여러 번 평가되는 이유는?
2. `SQUARE(i++)`는 어떤 tokens로 확장되는가?
3. 왜 C17 undefined behavior인가?
4. multiple evaluation이 항상 UB인가?
5. compiler warning이 없으면 안전하다고 볼 수 있는가?
## 12. 핵심 정리
- replacement의 parameter occurrence 수가 argument 평가 횟수에 영향을 준다.
- unsequenced modifications는 C17 UB다.
- 정상 예제에는 side-effect-free arguments만 사용한다.
## 13. 다음 Step
[25-7. `#if`, `#ifdef`, `#ifndef`, `#endif`](25-7-conditional-directives.md)
## 14. 참고 자료
- N1570 5.1.2.3, 6.5, 6.10.3.1. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: Order of evaluation](https://en.cppreference.com/w/c/language/eval_order)
- [GCC: Duplication of Side Effects](https://gcc.gnu.org/onlinedocs/cpp/Duplication-of-Side-Effects.html)
