# 10-8. call stack 기초

call stack은 많은 구현에서 활성 함수 호출의 복귀 위치와 필요한 상태를 관리하는 모델이지만 C17이 특정 memory 구조를 요구하지는 않는다.

## 1. 학습 목표
- caller와 called function의 제어 흐름을 추적한다.
- stack frame을 구현 모델로 설명한다.
- C 표준과 ABI·CPU 구현을 구분한다.

## 2. 선수 지식
Step 10-1의 호출·복귀와 Step 10-6의 호출별 automatic object를 안다.

## 3. 핵심 개념
`main`이 `double_value`를 호출하면 caller인 `main`의 다음 실행 지점을 보존하고 called function body를 실행한 뒤 돌아온다. 일반적인 ABI는 호출마다 stack frame을 둘 수 있지만, parameter와 local 값은 register에 있거나 최적화로 사라질 수 있다. 따라서 “모든 함수가 동일한 stack frame을 만든다”는 C17 규칙이 아니다.

## 4. 문법
```c
int caller(void)
{
    int result = called_function(3);
    return result;
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int double_value(int value)
{
    printf("inside\n");
    return value * 2;
}

int main(void)
{
    printf("before\n");
    int result = double_value(3);
    printf("after: %d\n", result);
    return 0;
}
```

## 6. 코드 해석
출력 순서는 `before`, `inside`, `after: 6`이다. 호출 중에는 `double_value`가 실행되고 반환 후에는 `main`의 호출식 다음 statement가 계속된다.

## 7. 내부 동작
[C 언어 관점] 표준은 호출식 평가, parameter 초기화, body 실행, return을 규정한다. [컴파일러/ABI 관점] return address, register 보존, stack frame 형식은 ABI에 달린다. [CPU 관점] ISA별 call/return 명령이나 register가 다르며 compiler가 함수를 inline하면 실제 call instruction이 없을 수도 있다.

## 8. 자주 하는 실수
- call stack을 C abstract machine의 필수 자료구조라고 단정한다.
- 모든 arguments와 locals가 stack에 놓인다고 말한다.
- 함수 호출이 다른 source file로 이동하는 것이라고 설명한다.
- 출력 순서에서 called function body를 빼먹는다.

## 9. 필수 실습
호출 전·함수 내부·반환 후 메시지를 출력해 제어 흐름을 확인한다. [실습 README](../../exercises/10-functions/10-8/README.md)

## 10. 추가 실습
- ★ 함수 두 개를 차례로 호출
- ★★ 함수 A가 함수 B를 호출하는 순서 추적
- ★★★ C17 보장과 ABI 구현을 표로 구분

## 11. 확인 문제
1. caller란 무엇인가?
2. called function이 return하면 어디서 계속하는가?
3. C17은 stack frame 형식을 정하는가?
4. parameter가 항상 stack에 있는가?
5. compiler가 call instruction을 없앨 수 있는 경우는?

## 12. 핵심 정리
호출은 제어가 함수로 갔다가 caller로 돌아오는 언어 동작이며, call stack의 구체적 형태는 구현과 ABI의 영역이다.

## 13. 다음 Step
[Step 10-9. 계산기 연산 함수 분리](10-9-calculator-functions.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 5.1.2.3, 6.5.2.2, 6.9.1
- [System V AMD64 ABI](https://gitlab.com/x86-psABIs/x86-64-ABI): 구현 예시
- [cppreference: Function definition](https://en.cppreference.com/w/c/language/function_definition.html)
