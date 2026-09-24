# 25-9. macro·함수·상수 선택
## 1. 학습 목표
- preprocessing macro, function, enum constant, `const` object를 목적에 맞게 선택한다.
- type checking과 evaluation 횟수를 비교한다.
- 모든 named value를 macro로 만들지 않는다.
## 2. 선수 지식
25-3부터 25-8까지와 Part 10·17·20을 안다.
## 3. 핵심 개념
| 목적 | 우선 검토 |
|---|---|
| conditional preprocessing 제어 | macro |
| typed 계산, argument 한 번 평가 | function |
| integer constant expression | enum constant |
| typed read-only object | `const` object |

macro가 유일한 선택인 경우도 있지만 type과 runtime semantics가 필요하면 C language mechanism을 먼저 검토한다.
## 4. 문법
```c
#define FEATURE_LEVEL 2

enum { BUFFER_SIZE = 4 };

static int square_int(int value)
{
    return value * value;
}
```
## 5. 최소 코드 예제
`main.c`
```c
#include <stdio.h>

#define FEATURE_LEVEL 2

enum { BUFFER_SIZE = 4 };

static int square_int(int value)
{
    return value * value;
}

int main(void)
{
    const double rate = 1.5;
    int values[BUFFER_SIZE] = {1, 2, 3, 4};

#if FEATURE_LEVEL >= 2
    printf("%d %.1f %zu\n",
           square_int(values[2]),
           rate,
           sizeof values / sizeof values[0]);
#endif
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    main.c -o choice_app
./choice_app
```
## 6. 코드 해석
`FEATURE_LEVEL`은 preprocessing selection에, `BUFFER_SIZE`는 integer constant expression에, `rate`는 typed object에, 제곱은 typed function에 맡긴다.
## 7. 내부 동작
**[C translation phases / preprocessing]** `FEATURE_LEVEL` macro가 `#if` controlling expression에 사용된다.

**[C compiler]** enum constant, `const` object, function parameter·return type을 C type system으로 검사한다.

**[GCC driver / option]** warning options는 expansion 뒤 code의 문제를 찾는 데 도움을 주지만 macro safety를 전부 보장하지 않는다.

**[build system]** public configuration macro는 이름·허용값·build interface를 명확히 정해야 한다.

**[OS / CPU]** enum constant는 별도 runtime object를 요구하지 않고 `const` object와 function은 번역 결과에 따라 runtime 동작에 참여한다.
## 8. 자주 하는 실수
- 모든 상수와 계산을 macro로 만든다.
- macro가 function처럼 type checking과 single evaluation을 제공한다고 생각한다.
- C의 file-scope `const` linkage를 C++ 규칙으로 설명한다.
- enum constant와 typed object를 같은 storage object라고 부른다.
- overly generic macro names로 다른 headers와 충돌한다.
## 9. 필수 실습
configuration에는 macro, array bound에는 enum, read-only value에는 `const`, 계산에는 function을 사용한다.
[25-9 exercise](../../exercises/25-preprocessor/25-9/README.md)
## 10. 추가 실습
- ★ `BUFFER_SIZE`를 5로 바꾸고 initializer를 맞춘다.
- ★★ `square_int`에 side-effect expression을 전달해도 한 번만 평가되는 이유를 설명한다.
- ★★★ 선택 기준을 project coding guideline 한 문단으로 작성한다.
## 11. 확인 문제
1. conditional compilation에는 무엇이 필요한가?
2. integer constant expression이 필요할 때 enum의 장점은?
3. function이 macro보다 type·evaluation 면에서 안전한 이유는?
4. `const` object와 macro의 storage 차이는?
5. 모든 named constant를 macro로 만들 필요가 없는 이유는?
## 12. 핵심 정리
- preprocessing이 필요한 곳에만 macro를 사용한다.
- typed calculation에는 function을 우선한다.
- enum constant와 `const` object의 서로 다른 역할을 활용한다.
## 13. 다음 Step
[25-10. Part 25 종합 복습](25-10-part-25-review.md)
## 14. 참고 자료
- N1570 6.2.2, 6.4.4, 6.7.3, 6.10.3. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: Constant expression](https://en.cppreference.com/w/c/language/constant_expression)
- [cppreference: `const` type qualifier](https://en.cppreference.com/w/c/language/const)
