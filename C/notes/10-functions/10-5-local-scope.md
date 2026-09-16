# 10-5. 지역 변수와 scope

scope는 identifier를 소스 코드의 어느 영역에서 사용할 수 있는지 정한다.

## 1. 학습 목표
- parameter와 함수 본문 local variable의 scope를 찾는다.
- 서로 다른 block의 같은 이름을 별도 identifier로 구분한다.
- scope와 lifetime을 같은 개념으로 취급하지 않는다.

## 2. 선수 지식
Part 2의 변수 선언과 Step 10-3의 parameter를 안다.

## 3. 핵심 개념
함수 definition의 parameter는 선언 뒤부터 함수 definition 전체에서 사용할 수 있다. 함수 body 안에서 선언한 local variable은 그 선언 지점부터 자신을 포함하는 block 끝까지 유효하다. 바깥 block의 이름을 안쪽 block에서 다시 선언하면 안쪽 이름이 바깥 이름을 가린다.

## 4. 문법
```c
int calculate(int parameter)
{
    int local = parameter + 1;
    return local;
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int double_value(int value)
{
    int result = value * 2;
    return result;
}

int main(void)
{
    int value = 5;
    printf("%d\n", double_value(value));
    printf("%d\n", value);
    return 0;
}
```

## 6. 코드 해석
`main`의 `value`와 `double_value`의 parameter `value`는 이름만 같고 scope가 다른 객체다. 함수 안의 `result`는 그 함수 body 안에서만 이름으로 사용할 수 있다. 출력은 10과 5다.

## 7. 내부 동작
[C17 표준] parameter와 body 최상위 block의 local variable은 function body 안에서 block scope 규칙을 따른다. scope는 이름의 가시 영역이며 객체가 실제로 존재하는 기간인 lifetime과 구분된다. linkage와 전체 storage duration 분류는 후속 Part에서 더 자세히 다룬다.

## 8. 자주 하는 실수
- 다른 함수의 local variable을 이름으로 직접 사용하려 한다.
- 같은 이름이면 같은 객체라고 생각한다.
- scope가 끝났다는 말과 값이 0으로 초기화된다는 말을 혼동한다.
- 불필요하게 같은 이름을 중첩 선언해 가림을 만든다.

## 9. 필수 실습
caller와 called function에 같은 이름의 변수를 두고 두 값이 독립적임을 출력한다. [실습 README](../../exercises/10-functions/10-5/README.md)

## 10. 추가 실습
- ★ 함수 안 local 결과 변수 사용
- ★★ 안쪽 block에서 이름 가림 관찰
- ★★★ 각 identifier의 scope를 코드에 표시

## 11. 확인 문제
1. parameter 이름은 함수의 어디에서 사용할 수 있는가?
2. local variable의 scope는 언제 시작하는가?
3. 두 함수의 같은 변수 이름은 같은 객체인가?
4. scope와 lifetime은 같은 용어인가?

## 12. 핵심 정리
scope는 이름을 사용할 수 있는 소스 영역이며, 함수마다 parameter와 local variable은 독립적으로 관리된다.

## 13. 다음 Step
[Step 10-6. 자동 객체의 lifetime](10-6-automatic-lifetime.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.2.1, 6.9.1
- [cppreference: Scope](https://en.cppreference.com/w/c/language/scope.html)
