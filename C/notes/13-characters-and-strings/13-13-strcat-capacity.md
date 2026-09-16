# 13-13. `strcat`과 연결 용량

`strcat`은 destination의 기존 terminator 위치부터 source string을 terminator까지 이어 붙인다.

## 1. 학습 목표
- 연결 후 필요한 destination capacity를 계산한다.
- destination과 source의 valid string 전제를 설명한다.
- overlap과 overflow 위험을 피한다.

## 2. 선수 지식
Step 13-10의 `strlen`과 Step 13-11의 destination capacity를 안다.

## 3. 핵심 개념
필요한 capacity는 `strlen(destination) + strlen(source) + 1`이다. 마지막 1은 결과 null terminator 자리다. `strcat`은 destination의 실제 array capacity를 받지 않으므로 caller가 충분한 공간을 보장해야 한다.

## 4. 문법
```c
#include <string.h>
strcat(destination, source);
```

## 5. 최소 코드 예제
```c
#include <stdio.h>
#include <string.h>

int main(void)
{
    char destination[12] = "hello";
    char source[] = " world";

    strcat(destination, source);
    printf("%s\n", destination);
    return 0;
}
```

## 6. 코드 해석
기존 length 5와 source length 6에 terminator 1을 더해 capacity 12가 필요하다. 연결 결과는 `"hello world"`이며 마지막 element에 null character가 있다.

## 7. 내부 동작
[C17 표준] `strcat`은 destination terminator를 찾고 source를 terminator까지 덧붙인다. destination 공간이 부족하거나 source와 destination이 overlap하면 Undefined Behavior다. 자동 capacity 검사는 제공되지 않는다.

## 8. 자주 하는 실수
- 두 lengths만 더하고 terminator 공간을 빼먹는다.
- destination을 literal 내용과 정확히 같은 작은 배열로 둔다.
- `strcat`이 capacity를 검사한다고 생각한다.
- overflow나 overlap을 실행해 결과를 관찰한다.

## 9. 필수 실습
두 짧은 strings의 lengths를 계산해 충분한 고정 destination에서 연결한다. [실습 README](../../exercises/13-characters-and-strings/13-13/README.md)

## 10. 추가 실습
- ★ empty source 연결
- ★★ empty destination에 연결
- ★★★ 여러 조합의 최소 capacity 계산

## 11. 확인 문제
1. 필요한 capacity 식은?
2. 마지막 +1의 의미는?
3. `strcat`이 capacity argument를 받는가?
4. destination이 작으면 어떤 문제인가?
5. overlap을 허용하는가?

## 12. 핵심 정리
string concatenation은 두 lengths와 terminator를 모두 수용하는 destination capacity와 non-overlap contract가 필요하다.

## 13. 다음 Step
[Step 13-14. 선택 실습: `my_strcat`](13-14-my-strcat.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 7.24.3.1
- [cppreference: strcat](https://en.cppreference.com/w/c/string/byte/strcat.html)
