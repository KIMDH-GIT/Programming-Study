# 28-1. `-std=c17 -Wall -Wextra -Wpedantic`
## 1. 학습 목표
- GCC의 C dialect와 warning options 역할을 구분한다.
- `-Wall`, `-Wextra`, `-Wpedantic`이 활성화하는 진단 범위를 정확히 설명한다.
- warning 부재와 C17 correctness를 동일시하지 않는다.
## 2. 선수 지식
Part 0의 translation 과정과 Part 27의 diagnostic·Undefined Behavior 구분을 안다.
## 3. 핵심 개념
**[GCC]** `-std=c17`은 GCC가 사용할 C language dialect를 ISO C17 계열로 선택한다. program이 C17을 완벽히 준수하거나 모든 실행이 정확함을 증명하지 않는다.

`-Wall`은 이름과 달리 모든 warning을 켜지 않는다. GCC가 유용하고 비교적 피하기 쉽다고 정한 특정 warning 집합을 활성화한다. `-Wextra`는 `-Wall`에 포함되지 않은 추가 warning 집합을 활성화한다. 정확한 구성은 GCC version 문서로 확인한다.

`-Wpedantic`은 선택한 `-std`의 base standard가 요구하는 진단과 금지된 extension 사용 등에 대한 pedantic warnings를 요청한다. 모든 non-standard code를 무조건 error로 만드는 option은 아니다.
## 4. 문법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic main.c -o app
```

학습·CI에서는 warning을 놓치지 않도록 다음을 추가할 수 있다.
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o app
```

**[GCC]** `-Werror`는 warning을 error로 취급하게 한다. ISO C17이 요구하는 mode가 아니며 compiler version 변화로 새 warning이 build를 막을 수 있다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

static int sum(const int values[], size_t count)
{
    int result = 0;

    for (size_t i = 0; i < count; ++i) {
        result += values[i];
    }
    return result;
}

int main(void)
{
    int values[] = {1, 2, 3};

    printf("%d\n", sum(values, sizeof values / sizeof values[0]));
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o warning_options
./warning_options
```
## 6. 코드 해석
배열 길이를 함께 전달하고 유효한 범위만 순회한다. strict warning build를 통과하고 `6`을 출력하지만, 그 사실만으로 모든 입력과 모든 semantic property가 증명되는 것은 아니다.
## 7. 내부 동작
**[C17]** source의 syntax·constraints·semantics를 규정하며 exact GCC option이나 diagnostic 문구를 정의하지 않는다.

**[GCC]** dialect를 선택하고 translation-time analysis로 diagnostics를 낸다. warning 집합과 wording은 version에 따라 달라질 수 있다.

**[OS / ABI / CPU]** executable 실행과 machine code는 이후 층이며 warning option 자체가 runtime check를 추가하는 것은 아니다.
## 8. 자주 하는 실수
- `-Wall`을 “모든 warning”이라고 풀어 쓴다.
- `-Wextra`가 이미 `-Wall`에 모두 포함된다고 생각한다.
- `-Wpedantic`이 모든 extension을 반드시 거부한다고 한다.
- `-std=c17`과 warning 통과를 correctness proof로 사용한다.
## 9. 필수 실습
네 option의 역할을 표로 정리하고 정상 예제를 compile·run한다.
[28-1 exercise](../../exercises/28-debugging/28-1/README.md)
## 10. 추가 실습
- ★ option을 하나씩 추가하며 command를 기록한다.
- ★★ GCC manual에서 현재 `-Wall` 구성 일부를 찾는다.
- ★★★ required diagnostic과 optional warning을 비교한다.
## 11. 확인 문제
1. `-std=c17`은 무엇을 선택하는가?
2. `-Wall`이 모든 warning을 뜻하지 않는 이유는?
3. `-Wextra`의 역할은?
4. `-Wpedantic`과 compile rejection은 왜 같은 말이 아닌가?
5. `-Werror`는 C17 requirement인가?
## 12. 핵심 정리
- C17 semantics와 GCC options를 별도 층으로 본다.
- warning 집합은 유용하지만 완전한 bug 검출기가 아니다.
- exact option semantics는 사용 중인 GCC 공식 문서로 확인한다.
## 13. 다음 Step
[28-2. compiler warning 읽고 수정하기](28-2-reading-and-fixing-compiler-warnings.md)
## 14. 참고 자료
- N1570 5.1.1.3. N1570은 **C11 공개 Committee Draft**이며 diagnostic requirement는 C17에서도 유지된다.
- [GCC: Warning Options](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html)
