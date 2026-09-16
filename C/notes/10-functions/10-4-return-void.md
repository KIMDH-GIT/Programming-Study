# 10-4. 반환값과 `void`

return type은 호출식이 만드는 값의 타입을 정하며, `void` 함수는 호출자에게 값을 반환하지 않는다.

## 1. 학습 목표
- `return expression;`과 `return;`의 적용 대상을 구분한다.
- return type의 `void`와 parameter list의 `void`를 구분한다.
- 반환 expression의 conversion을 설명한다.

## 2. 선수 지식
Step 10-1의 반환값과 Part 6의 implicit conversion을 사용한다.

## 3. 핵심 개념
값을 반환하는 함수는 `return expression;`을 사용하고 expression 값은 함수 return type으로 변환된다. `void` return type 함수에서는 `return;`으로 일찍 끝내거나 본문 끝에 도달할 수 있다. `void print_message(void)`에서 앞의 `void`는 반환값이 없다는 뜻이고 괄호 안의 `void`는 parameter가 없다는 뜻이다.

## 4. 문법
```c
int square(int value) { return value * value; }
void print_message(void) { printf("hello\n"); return; }
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

void print_message(void)
{
    printf("hello\n");
}

int square(int value)
{
    return value * value;
}

int main(void)
{
    print_message();
    printf("%d\n", square(4));
    return 0;
}
```

## 6. 코드 해석
`print_message()`는 문자열을 출력하지만 값은 만들지 않는다. `square(4)`는 16을 반환하고 그 호출식의 결과가 `printf` argument가 된다.

## 7. 내부 동작
[C17 표준] `return expression;`의 expression 값은 함수 return type으로 변환된다. 값을 반환하는 함수에서 expression 없는 `return;`은 constraint violation이며 diagnostic이 필요하다. `void` 함수에서 값을 반환하는 것도 constraint violation이다. `main`의 끝에 도달하는 특별 규칙은 `return 0;`과 같은 효과다.

## 8. 자주 하는 실수
- 두 위치의 `void`를 같은 문법 역할로 설명한다.
- `void` 호출의 결과를 변수에 저장한다.
- `int` 함수에서 모든 경로의 반환을 빠뜨린다.
- return type과 다른 expression이 항상 syntax error라고 단정한다.

## 9. 필수 실습
메시지를 출력하는 `void` 함수와 절댓값을 반환하는 `int` 함수를 각각 작성한다. [실습 README](../../exercises/10-functions/10-4/README.md)

## 10. 추가 실습
- ★ 인사말만 출력하는 함수
- ★★ 양수 여부를 0 또는 1로 반환
- ★★★ 잘못된 return 코드를 실행하지 말고 constraint 위반 분류

## 11. 확인 문제
1. return type의 `void`는 무엇을 뜻하는가?
2. parameter list의 `void`는 무엇을 뜻하는가?
3. `return;`은 어떤 함수에서 사용할 수 있는가?
4. 반환 expression에는 어떤 conversion이 일어날 수 있는가?
5. `main` 끝 도달의 특별 규칙은 무엇인가?

## 12. 핵심 정리
return type과 return statement를 맞추며, `void`의 반환 위치와 parameter 위치 의미를 따로 읽는다.

## 13. 다음 Step
[Step 10-5. 지역 변수와 scope](10-5-local-scope.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.8.6.4, 6.9.1
- [cppreference: return statement](https://en.cppreference.com/w/c/language/return.html)
- [cppreference: void type](https://en.cppreference.com/w/c/language/void.html)
