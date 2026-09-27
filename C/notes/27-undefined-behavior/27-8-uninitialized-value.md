# 27-8. uninitialized value
## 1. 학습 목표
- 초기화되지 않은 automatic object의 indeterminate value를 설명한다.
- unspecified value와 trap representation을 구분한다.
- 실제 사용 전에 모든 control path에서 값을 정한다.
## 2. 선수 지식
Part 2의 선언·초기화와 Part 10의 automatic object를 안다.
## 3. 핵심 개념
initializer 없이 선언된 automatic object는 indeterminate value를 가진다. C17의 indeterminate value는 unspecified value 또는 trap representation이다. 어떤 type·representation·operation인지에 따라 세부 결과가 미묘하므로 “초기화되지 않은 모든 byte read는 항상 같은 종류의 UB”라고 일반화하지 않는다.

이 Step에서는 대표적인 `int` 값을 계산이나 출력에 사용하는 코드를 잘못된 사례로 다룬다.

⚠ 분석용 — 실행하지 않는다.
```c
int value;
printf("%d\n", value); /* indeterminate int 사용: 실행하지 않는다 */
```
## 4. 문법
선언 시 초기화하거나 성공 상태와 함께 값을 전달한다.
```c
int value = 0;
if (read_value(&value)) {
    use(value);
}
```
## 5. 최소 코드 예제
```c
#include <stdio.h>

static int choose_value(int enabled, int *out)
{
    if (!enabled || out == NULL) {
        return 0;
    }
    *out = 42;
    return 1;
}

int main(void)
{
    int value = 0;

    if (choose_value(1, &value)) {
        printf("%d\n", value);
    }
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o initialized
./initialized
```
## 6. 코드 해석
`value`는 처음부터 0으로 초기화되고, 출력 path에서는 `choose_value`가 42를 저장했다. 출력은 `42`다.
## 7. 내부 동작
**[C17]** indeterminate value는 unspecified value 또는 trap representation이다. non-character type의 trap representation을 읽는 lvalue expression은 UB가 될 수 있다. library·type별 규칙을 함께 확인해야 한다.

**[compiler diagnostic]** data-flow warning은 유용하지만 모든 path와 UB를 완전하게 판정하지 않는다.

**[runtime observation]** 이전 stack byte처럼 보이는 값은 C17 결과가 아니며 “랜덤 값”이라는 정의도 아니다.

**[C23 이후]** 이후 표준의 초기화·indeterminate 관련 변경을 이 C17 교재의 규칙으로 가져오지 않는다.
## 8. 자주 하는 실수
- “초기화되지 않은 변수에는 랜덤 값이 들어 있다”고 정의한다.
- warning이 없으면 initialized라고 결론 내린다.
- zero-filled OS page를 C automatic initialization 보장으로 생각한다.
- indeterminate value, unspecified value, UB를 같은 말로 쓴다.
## 9. 필수 실습
출력 parameter를 성공한 path에서만 사용하고 declaration 시 안전한 초기값을 준다.
[27-8 exercise](../../exercises/27-undefined-behavior/27-8/README.md)
## 10. 추가 실습
- ★ 모든 branch에서 값을 정하는 함수를 만든다.
- ★★ 성공 flag와 output value의 관계를 표로 쓴다.
- ★★★ compiler data-flow warning과 C17 의미를 분리한다.
## 11. 확인 문제
1. indeterminate value의 두 가능성은?
2. unspecified value는 trap representation일 수 있는가?
3. 초기화되지 않은 값을 “랜덤 값”으로 정의하면 왜 부정확한가?
4. warning 부재가 초기화를 증명하는가?
5. OS zero page가 automatic initialization을 보장하는가?
## 12. 핵심 정리
- automatic object는 사용 전에 명시적으로 값을 정한다.
- indeterminate·unspecified·trap representation을 구분한다.
- 이 Step의 잘못된 `int` read는 실행하지 않는다.
## 13. 다음 Step
[27-9. invalid pointer arithmetic](27-9-invalid-pointer-arithmetic.md)
## 14. 참고 자료
- N1570 3.19.2-3.19.4, 6.2.4p2, 6.2.6.1p5, 6.7.9p10, Annex J.2. N1570은 **C11 공개 Committee Draft**이며 여기서 다루는 규칙은 C17에서도 유지된다.
- [cppreference: Objects and alignment](https://en.cppreference.com/w/c/language/object)
