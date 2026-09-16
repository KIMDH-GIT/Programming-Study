# 13-12. `strcmp`의 반환값

`strcmp`는 두 valid strings의 lexicographic 관계를 음수, 0, 양수로 반환한다.

## 1. 학습 목표
- `strcmp` result의 sign을 해석한다.
- equal 판단에 0을 사용한다.
- 반환값을 -1·0·1로 한정하지 않는다.

## 2. 선수 지식
Step 13-9의 `my_strcmp`와 Part 8의 조건 분기를 사용한다.

## 3. 핵심 개념
`strcmp(a,b) == 0`이면 contents가 equal이다. 결과가 음수면 a가 b보다 before, 양수면 after다. C17은 정확한 negative/positive magnitude를 정하지 않으므로 `== -1`이나 `== 1`로 검사하면 안 된다.

## 4. 문법
```c
int result = strcmp(first, second);
if (result < 0) { /* before */ }
else if (result > 0) { /* after */ }
else { /* equal */ }
```

## 5. 최소 코드 예제
```c
#include <stdio.h>
#include <string.h>

int main(void)
{
    char first[] = "cat";
    char second[] = "cat";
    int result = strcmp(first, second);

    if (result == 0) {
        printf("equal\n");
    } else if (result < 0) {
        printf("before\n");
    } else {
        printf("after\n");
    }
    return 0;
}
```

## 6. 코드 해석
두 strings의 contents가 모두 같고 terminator 위치도 같아 result는 0이다. 프로그램은 `equal`을 출력한다.

## 7. 내부 동작
[C17 표준] `strcmp`는 처음 다른 character pair를 unsigned char로 해석한 차이의 sign에 따른 값을 반환한다. 정확한 -1 또는 1을 보장하지 않는다. execution character set이 전체 alphabet ordering을 ASCII와 동일하게 만든다고 일반화하지 않는다.

## 8. 자주 하는 실수
- result가 반드시 -1, 0, 1이라고 생각한다.
- arrays를 `==`로 내용 비교한다.
- `strcmp` result 자체를 boolean equal처럼 사용한다.
- valid null-terminated strings가 아닌 inputs를 넘긴다.

## 9. 필수 실습
equal·before·after pairs를 `strcmp`로 비교하고 sign으로 분류한다. [실습 README](../../exercises/13-characters-and-strings/13-12/README.md)

## 10. 추가 실습
- ★ equal pair
- ★★ prefix pair
- ★★★ result magnitude에 의존하는 잘못된 코드 분석

## 11. 확인 문제
1. equal result는?
2. negative result 의미는?
3. positive result 의미는?
4. 정확히 -1과 1을 보장하는가?
5. `a == b`로 contents를 비교할 수 있는가?

## 12. 핵심 정리
`strcmp`는 result의 sign만 해석하며 0이 content equality를 뜻한다.

## 13. 다음 Step
[Step 13-13. `strcat`과 연결 용량](13-13-strcat-capacity.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 7.24.4.2
- [cppreference: strcmp](https://en.cppreference.com/w/c/string/byte/strcmp.html)
