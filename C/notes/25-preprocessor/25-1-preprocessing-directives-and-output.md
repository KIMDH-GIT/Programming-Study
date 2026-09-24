# 25-1. 전처리 지시문과 결과
## 1. 학습 목표
- preprocessing이 translation 과정의 일부임을 설명한다.
- preprocessing directive와 runtime statement를 구분한다.
- GCC `-E`로 preprocessing 결과를 관찰한다.
## 2. 선수 지식
Part 0의 translation 과정과 Part 24의 translation unit을 안다.
## 3. 핵심 개념
전처리는 program 실행 전에 CPU가 수행하는 runtime 기능이 아니라 C source를 번역하는 과정의 일부다.

```text
source code
  ↓ preprocessing
preprocessed translation unit
  ↓ compilation·assembly
object code
  ↓ link
executable
  ↓ OS loader
execution
```

`#include`, `#define`, `#if`는 모두 preprocessing directives지만 source inclusion, macro definition, conditional inclusion이라는 서로 다른 역할을 한다.
## 4. 문법
```c
#include <stdio.h>
#define MESSAGE "preprocessing"

#if 1
/* 이 token group은 translation에 포함된다. */
#endif
```

directive는 C runtime statement가 아니므로 끝에 statement용 semicolon을 붙이는 규칙도 없다.
## 5. 최소 코드 예제
`main.c`
```c
#include <stdio.h>

#define MESSAGE "preprocessing"

int main(void)
{
    puts(MESSAGE);
    return 0;
}
```

preprocessing 결과 관찰:
```sh
gcc -std=c17 -E main.c
```

정상 build와 실행:
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    main.c -o preprocessor_app
./preprocessor_app
```
## 6. 코드 해석
preprocessor는 `stdio.h`의 내용을 source stream에 포함하고 `MESSAGE` preprocessing token을 replacement list의 tokens로 바꾼다. 그 결과를 compiler가 C program으로 분석한다.
## 7. 내부 동작
**[C translation phases / preprocessing]** source file은 translation phases를 거친다. C17의 순서에서 backslash-newline splicing은 phase 2, preprocessing token 구분과 comments의 space 대체는 phase 3, preprocessing directives와 macro expansion은 phase 4에서 처리된다.

**[C compiler]** preprocessing 결과의 declarations, expressions, types를 검사하고 code를 생성한다.

**[GCC driver / option]** `-E`는 preprocessing 뒤 멈추는 GCC driver option이며 C17 source syntax가 아니다. 출력에는 GCC line markers가 포함될 수 있다.

**[build system]** build command는 필요한 source마다 preprocessing·compilation을 요청한다.

**[OS / CPU]** executable이 완성된 뒤 loading과 instruction execution에 관여한다. directives를 runtime에 한 줄씩 실행하지 않는다.
## 8. 자주 하는 실수
- preprocessor를 CPU runtime 기능이라고 설명한다.
- `#define`을 variable assignment statement라고 부른다.
- `#`로 시작하는 모든 directive를 같은 종류의 macro라고 부른다.
- macro expansion을 임의의 character string 치환으로만 설명한다. C에서는 preprocessing tokens와 정해진 expansion rules가 핵심이다.
- `gcc -E` 성공을 type checking·link 성공의 증거로 삼는다.
## 9. 필수 실습
`MESSAGE` macro가 있는 source를 `gcc -E`로 관찰한 뒤 strict C17로 build·실행한다.
[25-1 exercise](../../exercises/25-preprocessor/25-1/README.md)
## 10. 추가 실습
- ★ replacement list를 다른 string literal로 바꾼다.
- ★★ `-E` 출력에서 `main`과 확장된 string literal을 찾는다.
- ★★★ preprocessing, compilation, link, execution의 입력·출력을 표로 만든다.
## 11. 확인 문제
1. preprocessing은 runtime 과정인가?
2. preprocessing directive와 C statement의 차이는?
3. `#include`, `#define`, `#if`의 역할은 각각 무엇인가?
4. GCC `-E`는 C17 문법인가?
5. `-E` 성공만으로 type checking과 link 성공을 알 수 없는 이유는?
## 12. 핵심 정리
- preprocessing은 C translation 과정의 일부다.
- directives는 runtime statements가 아니다.
- GCC `-E` 출력과 conceptual preprocessing result를 구분한다.
## 13. 다음 Step
[25-2. `#include`](25-2-include.md)
## 14. 참고 자료
- N1570 5.1.1.2, 6.10. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: Phases of translation](https://en.cppreference.com/w/c/language/translation_phases)
- [GCC: Preprocessor Options](https://gcc.gnu.org/onlinedocs/gcc/Preprocessor-Options.html)
