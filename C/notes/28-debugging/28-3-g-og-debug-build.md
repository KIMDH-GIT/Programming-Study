# 28-3. `-g -Og` debug build
## 1. 학습 목표
- `-g`와 `-Og`의 서로 다른 목적을 설명한다.
- source-level debugging용 build를 만든다.
- optimized build에서 debugger 관찰이 달라질 수 있음을 안다.
## 2. 선수 지식
28-1의 GCC options와 Part 0의 compile·link 과정을 안다.
## 3. 핵심 개념
**[GCC]** `-g`는 debugger가 활용할 debug information 생성을 요청한다. C semantics를 “debug mode”로 바꾸거나 optimization을 끄는 option이 아니다.

`-Og`는 debugging experience를 고려한 optimization level이다. GCC는 표준 edit-compile-debug cycle에 적절한 optimization과 debug information tracking의 균형으로 설명한다. `-O0`은 대부분의 optimization passes를 끄지만 UB를 defined로 만들지 않는다.
## 4. 문법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    -g -Og main.c -o debug_app
```

`-g`와 `-O0`, `-Og`, `-O2`는 독립 목적의 options다. optimized build에서는 변수의 storage가 사라지거나 `<optimized out>`으로 보이고, source line과 instruction 대응이 달라질 수 있다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

static int double_value(int value)
{
    int result = value * 2;
    return result;
}

int main(void)
{
    int answer = double_value(21);

    printf("%d\n", answer);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    -g -Og main.c -o debug_build
./debug_build
```
## 6. 코드 해석
모든 arithmetic이 표현 가능하고 output은 `42`다. debug information을 포함한 현재 executable을 GDB가 source와 연결해 사용할 수 있다.
## 7. 내부 동작
**[C17]** `answer`의 type·lifetime과 expression semantics를 규정한다.

**[GCC]** source line, symbol, type 등의 debug information을 target format으로 생성한다. `-Og`에서도 일부 최적화가 수행된다.

**[GDB]** debug information을 읽어 source-level stepping과 variable inspection을 제공한다.

**[OS / ABI / CPU]** 실제 executable format, register, stack frame은 구현 관찰이다.
## 8. 자주 하는 실수
- `-g`를 optimization off라고 설명한다.
- `-Og`를 “아무 최적화 없음”으로 설명한다.
- `-O0`이면 UB가 사라진다고 생각한다.
- source 한 줄이 machine instruction 하나라고 가정한다.
## 9. 필수 실습
같은 source를 `-g -Og`와 `-g -O0`로 build하고 output은 같되 debugger 관찰은 달라질 수 있음을 기록한다.
[28-3 exercise](../../exercises/28-debugging/28-3/README.md)
## 10. 추가 실습
- ★ `file` 또는 `readelf`로 debug section 존재를 관찰한다.
- ★★ `-O0`과 `-Og` GDB variable 관찰을 비교한다.
- ★★★ `-O2`에서 optimized-out 가능성을 설명한다.
## 11. 확인 문제
1. `-g`의 역할은?
2. `-g`가 optimization을 끄는가?
3. `-Og`는 어떤 목표의 level인가?
4. optimized-out은 C object lifetime이 없었다는 뜻인가?
5. debug/release는 C17 용어인가?
## 12. 핵심 정리
- `-g`는 debug information, `-Og`는 optimization level이다.
- 둘을 함께 사용해 source-level debugging build를 만든다.
- debugger 관찰과 C abstract machine 규칙을 분리한다.
## 13. 다음 Step
[28-4. AddressSanitizer build와 실행](28-4-addresssanitizer-build-and-run.md)
## 14. 참고 자료
- [GCC: Debugging Options](https://gcc.gnu.org/onlinedocs/gcc/Debugging-Options.html)
- [GCC: Optimize Options](https://gcc.gnu.org/onlinedocs/gcc/Optimize-Options.html)
