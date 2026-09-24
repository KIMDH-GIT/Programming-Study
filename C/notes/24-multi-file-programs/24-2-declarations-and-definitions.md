# 24-2. declaration과 definition
## 1. 학습 목표
- declaration과 definition을 구분한다.
- function prototype과 function body의 역할을 설명한다.
- 여러 translation units가 compatible declaration을 공유하게 한다.
## 2. 선수 지식
24-1 source 분리와 Part 10의 function prototype을 안다.
## 3. 핵심 개념
declaration은 identifier와 type 정보를 compiler에게 알린다. definition은 function body를 제공하거나 object를 정의한다.

```c
int calculator_add(int lhs, int rhs);       /* declaration */
int calculator_add(int lhs, int rhs)        /* definition */
{
    return lhs + rhs;
}
```

function definition도 declaration이다. 그러나 body 없는 prototype은 function definition이 아니다.
## 4. 문법
```c
int calculate(int lhs, int rhs);  /* proper prototype */
```

`int calculate();`는 C17에서 “parameter가 없음”을 뜻하는 prototype이 아니므로 공개 interface에 사용하지 않는다.
## 5. 최소 코드 예제
`temperature.h`
```c
double celsius_to_fahrenheit(double celsius);
```

`temperature.c`
```c
#include "temperature.h"

double celsius_to_fahrenheit(double celsius)
{
    return celsius * 9.0 / 5.0 + 32.0;
}
```

`main.c`
```c
#include <stdio.h>

#include "temperature.h"

int main(void)
{
    printf("%.1f\n", celsius_to_fahrenheit(20.0));
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    main.c temperature.c -o temperature_app
./temperature_app
```
## 6. 코드 해석
두 source는 같은 header의 prototype을 본다. 만약 implementation을 `int celsius_to_fahrenheit(double)`처럼 잘못 정의하면 `temperature.c` 안에서 conflicting types 진단을 받을 수 있다.
## 7. 내부 동작
**[preprocessor]** header declaration을 각 source의 preprocessing 결과에 넣는다.

**[C translation unit]** compiler는 현재 translation unit에서 declaration과 사용·definition의 type 호환성을 검사한다.

**[compiler]** declaration만 보고 call instruction을 위한 code를 만들 수 있다.

**[linker]** 다른 translation unit에 있는 definition을 찾아 external reference를 해결한다.

**[OS / loader]** link가 성공한 실행 파일만 정상적인 실행 대상으로 적재한다.

**[CPU / ISA]** declaration 자체를 실행하지 않고, 완성된 machine code를 실행한다.
## 8. 자주 하는 실수
- prototype을 여러 `.c`에 수동 복사한다.
- declaration만 있으면 definition도 자동으로 생긴다고 생각한다.
- return type·parameter type이 다른 declaration과 definition을 쓴다.
- `int f();`를 parameter 없는 prototype이라고 설명한다.
- implementation `.c`에서 자신의 header를 include하지 않는다.
## 9. 필수 실습
온도 변환 함수의 declaration과 definition을 서로 다른 파일에 배치한다.
[24-2 exercise](../../exercises/24-multi-file-programs/24-2/README.md)
## 10. 추가 실습
- ★ 화씨를 섭씨로 바꾸는 함수를 추가한다.
- ★★ header prototype의 parameter names를 문서 역할에 맞게 개선한다.
- ★★★ 의도적으로 return type을 바꾸고 compile diagnostic을 기록한 뒤 복구한다.
## 11. 확인 문제
1. function declaration과 definition의 차이는?
2. function definition도 declaration인가?
3. declaration만 보고 call을 compile할 수 있는 이유는?
4. definition을 찾지 못하면 어느 단계에서 문제가 드러나는가?
5. 구현 `.c`가 자신의 header를 include해야 하는 이유는?
## 12. 핵심 정리
- declaration은 이름과 type을 알리고 definition은 entity를 제공한다.
- 공개 prototype은 한 header에서 공유한다.
- compile 성공과 link 성공은 서로 다른 조건이다.
## 13. 다음 Step
[24-3. header file과 공개 interface](24-3-headers-and-public-interface.md)
## 14. 참고 자료
- N1570 6.2.1, 6.2.7, 6.7, 6.9. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: Function declaration](https://en.cppreference.com/w/c/language/function_declaration)
- [cppreference: Definitions](https://en.cppreference.com/w/c/language/definitions)
