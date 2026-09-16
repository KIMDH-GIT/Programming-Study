# 10-10. `print_menu`와 입력·처리·출력 분리

메뉴 출력, 입력 확인, 계산 처리를 구분하면 프로그램의 제어 흐름과 각 함수의 책임이 선명해진다.

## 1. 학습 목표
- `print_menu`를 값이 필요 없는 `void` 함수로 작성한다.
- 입력·처리·출력 단계를 구분한다.
- 함수 하나에 불필요하게 여러 역할을 몰아넣지 않는다.

## 2. 선수 지식
Part 5의 `scanf` 반환값, Part 8의 메뉴 조건, Step 10-4와 10-9를 사용한다.

## 3. 핵심 개념
`print_menu`는 메뉴를 출력하기만 하므로 parameter와 반환값이 필요 없다. `main`은 메뉴를 호출하고 입력을 검증한 뒤 계산 함수를 호출하며 결과를 출력한다. 이 정도의 분리는 입문 단계에서 흐름을 드러내지만, 아직 header나 여러 source file 설계까지 확장하지 않는다.

## 4. 문법
```c
void print_menu(void);
int add(int left, int right);
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

void print_menu(void)
{
    printf("1. add\n");
}

int add(int left, int right)
{
    return left + right;
}

int main(void)
{
    int left;
    int right;

    print_menu();
    printf("two integers: ");
    if (scanf("%d %d", &left, &right) != 2) {
        printf("invalid input\n");
        return 1;
    }

    int result = add(left, right);
    printf("%d\n", result);
    return 0;
}
```

## 6. 코드 해석
메뉴 출력은 `print_menu`, 입력과 검증은 `main`, 계산은 `add`, 최종 출력은 다시 `main`이 담당한다. 두 정수를 올바르게 입력하면 합을 출력하고, 입력 변환이 두 번 성공하지 않으면 오류 문장과 종료 상태 1을 반환한다.

## 7. 내부 동작
[C17 표준] `scanf` 반환값은 성공한 변환 개수이며 `&left`, `&right`는 Part 5에서 배운 입력 대상 주소다. `print_menu` 호출은 값을 만들지 않는다. [설계 관점] 역할 분리는 C17 문법 규칙이 아니라 프로그램 이해와 변경을 쉽게 하는 구성 방법이다.

## 8. 자주 하는 실수
- `scanf` 반환값을 확인하지 않는다.
- 출력 전용 함수에 계산과 입력까지 넣는다.
- `void print_menu(void)`의 두 `void` 의미를 혼동한다.
- 아직 배우지 않은 여러 파일·header 구조를 필수 실습에 추가한다.

## 9. 필수 실습
메뉴 출력 함수와 덧셈 처리 함수를 분리하고 `main`에서 입력 검증과 결과 출력을 담당한다. [실습 README](../../exercises/10-functions/10-10/README.md)

## 10. 추가 실습
- ★ 뺄셈 메뉴 문장 추가
- ★★ `switch`로 덧셈·뺄셈 선택
- ★★★ 각 함수의 입력·출력 책임을 표로 작성

## 11. 확인 문제
1. `print_menu`의 return type이 `void`인 이유는?
2. 입력 성공 여부는 어떤 값으로 확인하는가?
3. 계산은 어느 함수가 담당하는가?
4. 결과 출력은 어느 단계인가?
5. header 분리를 이번 필수 실습에서 하지 않는 이유는?

## 12. 핵심 정리
출력 전용 `print_menu`, 입력 검증, 계산 함수, 결과 출력을 구분해 각 단계의 책임을 작게 유지한다.

## 13. 다음 Step
[Step 10-11. Part 10 종합 복습](10-11-part-10-review.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.2.2, 7.21.6.2
- [cppreference: scanf](https://en.cppreference.com/w/c/io/fscanf.html)
- [cppreference: Function declaration](https://en.cppreference.com/w/c/language/function_declaration.html)
