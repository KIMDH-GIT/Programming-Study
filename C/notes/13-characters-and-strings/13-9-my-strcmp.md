# 13-9. `my_strcmp`

`my_strcmp`은 두 valid strings를 앞에서부터 비교해 처음 다른 unsigned character 값 또는 terminator로 순서를 결정한다.

## 1. 학습 목표
- 두 strings를 같은 index에서 비교한다.
- equal·before·after 결과를 부호로 표현한다.
- plain `char` signedness 영향을 피한다.

## 2. 선수 지식
Step 13-3의 null termination, Step 13-6의 array parameter, Part 8의 조건문을 사용한다.

## 3. 핵심 개념
같은 characters가 이어지는 동안 index를 증가시킨다. 처음 다른 위치에서 두 값을 `unsigned char`로 해석해 비교한다. 둘 다 같은 위치에서 `'\0'`이면 equal이다. custom 함수는 -1, 0, 1을 반환하도록 만들 수 있지만 standard `strcmp`는 음수·0·양수만 보장한다.

## 4. 문법
```c
int my_strcmp(char left[], char right[]);
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int my_strcmp(char left[], char right[])
{
    size_t i = 0;
    while (left[i] == right[i] && left[i] != '\0') {
        ++i;
    }

    unsigned char left_value = (unsigned char)left[i];
    unsigned char right_value = (unsigned char)right[i];

    if (left_value < right_value) {
        return -1;
    }
    if (left_value > right_value) {
        return 1;
    }
    return 0;
}

int main(void)
{
    char first[] = "cat";
    char second[] = "ca";
    printf("%d\n", my_strcmp(first, second));
    return 0;
}
```

## 6. 코드 해석
indices 0과 1의 c, a는 같다. index 2에서 first는 t이고 second는 null character다. null character 값은 0이고 t의 값은 0이 아니므로 first가 after라 1을 출력한다.

## 7. 내부 동작
[C17 표준] standard `strcmp`는 characters를 `unsigned char`로 해석한 것처럼 비교하고 결과의 sign만 규정한다. C는 ASCII 전체 순서를 강제하지 않으므로 alphabetic order 설명을 특정 encoding과 섞지 않는다. 두 inputs는 valid strings여야 한다.

## 8. 자주 하는 실수
- arrays에 `==`를 써 string 내용을 비교한다.
- standard `strcmp`가 반드시 -1 또는 1을 반환한다고 생각한다.
- terminator를 고려하지 않는다.
- plain `char`의 음수 가능성을 무시한다.

## 9. 필수 실습
equal, before, after 세 pairs를 `my_strcmp`로 비교해 결과 sign을 확인한다. [실습 README](../../exercises/13-characters-and-strings/13-9/README.md)

## 10. 추가 실습
- ★ 같은 strings
- ★★ prefix 관계 `"cat"`과 `"catalog"`
- ★★★ 각 comparison index 추적

## 11. 확인 문제
1. comparison은 언제 멈추는가?
2. equal result는?
3. standard `strcmp`가 정확히 -1/0/1을 보장하는가?
4. `unsigned char` 해석을 사용하는 이유는?
5. `a == b`가 string 내용 비교인가?

## 12. 핵심 정리
string comparison은 first differing unsigned character 또는 terminator를 찾아 결과의 sign으로 관계를 표현한다.

## 13. 다음 Step
[Step 13-10. `strlen`과 `sizeof`](13-10-strlen-sizeof.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 7.24.4.2
- [cppreference: strcmp](https://en.cppreference.com/w/c/string/byte/strcmp.html)
