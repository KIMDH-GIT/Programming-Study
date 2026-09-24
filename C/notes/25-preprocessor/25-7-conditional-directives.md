# 25-7. `#if`, `#ifdef`, `#ifndef`, `#endif`
## 1. 학습 목표
- conditional preprocessing과 runtime `if`를 구분한다.
- `#if`, `#ifdef`, `#ifndef`, `defined`를 정확히 사용한다.
- 서로 다른 macro configurations를 각각 build한다.
## 2. 선수 지식
25-3 object-like macro와 Part 8 runtime conditions를 안다.
## 3. 핵심 개념
conditional preprocessing은 어떤 token groups가 translation에 포함될지 결정한다. 제외된 group이 executable의 runtime branch로 남는 것은 아니다.

```c
#ifdef DEBUG
/* DEBUG macro가 정의된 configuration에 포함 */
#endif
```

`DEBUG`라는 runtime boolean variable을 검사하는 코드가 아니다.
## 4. 문법
```c
#if expression
#elif expression
#else
#endif

#ifdef NAME
#ifndef NAME

#if defined(NAME)
#if defined NAME
```

`defined`는 preprocessing conditional expression에서 쓰는 unary operator이며 ordinary C expression operator가 아니다.
## 5. 최소 코드 예제
`main.c`
```c
#include <stdio.h>

#ifndef FEATURE_LEVEL
#define FEATURE_LEVEL 1
#endif

#ifdef DEBUG
#define BUILD_MODE "debug"
#else
#define BUILD_MODE "release"
#endif

#if FEATURE_LEVEL >= 2
#define FEATURE_NAME "advanced"
#else
#define FEATURE_NAME "basic"
#endif

int main(void)
{
    printf("%s %s\n", BUILD_MODE, FEATURE_NAME);
    return 0;
}
```

두 configuration build:
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    main.c -o app_release
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    -DDEBUG -DFEATURE_LEVEL=2 main.c -o app_debug
./app_release
./app_debug
```
## 6. 코드 해석
default configuration은 `release basic`, command-line macros를 준 configuration은 `debug advanced`를 출력한다. `-D`와 `-DNAME=value`는 GCC driver options이지 ISO C source syntax가 아니다.
## 7. 내부 동작
**[C translation phases / preprocessing]** macro replacement와 `defined` 처리가 적용된 뒤 `#if` controlling expression이 평가된다. macro replacement 뒤 남은 identifiers는 이 문맥에서 `0`으로 대체되는 규칙이 있다.

**[C compiler]** 선택된 token groups만 각 configuration의 normal C translation 대상으로 검사한다.

**[GCC driver / option]** `-DDEBUG`와 `-DFEATURE_LEVEL=2`가 translation 시작 전 macro definitions를 제공한다.

**[build system]** 지원하는 configurations마다 별도 compile이 필요하다. 한 configuration 성공이 다른 configuration을 보장하지 않는다.

**[OS / CPU]** 선택되지 않은 source group을 runtime condition으로 실행하지 않는다.
## 8. 자주 하는 실수
- `#ifdef`를 runtime `if`라고 설명한다.
- `defined`를 일반 C operator라고 말한다.
- `-D`를 C17 source syntax라고 말한다.
- 미정의 `FEATURE_LEVEL`을 C variable lookup으로 생각한다.
- DEBUG build만 검사하고 normal build를 검사하지 않는다.
## 9. 필수 실습
default와 DEBUG·FEATURE_LEVEL=2 configurations를 각각 build·실행한다.
[25-7 exercise](../../exercises/25-preprocessor/25-7/README.md)
## 10. 추가 실습
- ★ `FEATURE_LEVEL=3`으로 build한다.
- ★★ `#ifdef DEBUG`를 `#if defined(DEBUG)`로 바꿔 같은 결과를 확인한다.
- ★★★ runtime feature flag와 compile-time selection의 trade-off를 정리한다.
## 11. 확인 문제
1. conditional preprocessing과 runtime `if`의 차이는?
2. `#ifdef DEBUG`가 검사하는 것은?
3. `defined`는 ordinary C operator인가?
4. 미정의 identifier는 `#if` expression에서 어떻게 처리되는가?
5. 두 configurations를 각각 compile해야 하는 이유는?
6. GCC `-D`와 ISO C source directive의 차이는?
## 12. 핵심 정리
- conditional directives는 translation에 포함할 token groups를 선택한다.
- `#ifdef`는 macro definition state를 검사한다.
- 모든 지원 configurations를 각각 compile한다.
## 13. 다음 Step
[25-8. header guard](25-8-header-guards.md)
## 14. 참고 자료
- N1570 6.10.1, 6.10.8. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: Conditional inclusion](https://en.cppreference.com/w/c/preprocessor/conditional)
- [GCC: Preprocessor Options (`-D`)](https://gcc.gnu.org/onlinedocs/gcc/Preprocessor-Options.html)
