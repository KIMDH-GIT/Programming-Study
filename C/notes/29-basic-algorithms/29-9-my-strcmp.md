# 29-9. `my_strcmp`
## 1. 학습 목표
- 두 C string을 unsigned character 값 기준으로 비교한다.
- 첫 차이 또는 terminator에서 comparison을 종료한다.
- subtraction overflow 없는 normalized result를 반환한다.
## 2. 선수 지식
29-7의 string contract와 Part 13의 character representation을 안다.
## 3. 핵심 개념
**[Algorithm]** 같은 위치의 character가 같으면 다음 위치로 진행한다. 첫 차이에서 두 unsigned character의 ordering을 비교한다. 둘 다 `'\0'`이면 같은 string이다.

결과 contract:

```text
left < right  → negative
left == right → zero
left > right  → positive
```

정확히 `-1`, `0`, `1`을 반환하는 것은 이 구현의 추가 contract다.
## 4. 문법
```c
return (left_value > right_value) - (left_value < right_value);
```

`return a - b;`를 임의 integer comparator에 일반화하면 signed overflow 가능성이 있다. relational result 차이는 `-1`, `0`, `1` 범위다.
## 5. 최소 코드 예제
```c
#include <stddef.h>
#include <stdio.h>

static int my_strcmp(const char left[], const char right[])
{
    size_t i = 0;

    while (left[i] != '\0' &&
           (unsigned char)left[i] == (unsigned char)right[i]) {
        ++i;
    }
    {
        unsigned char left_value = (unsigned char)left[i];
        unsigned char right_value = (unsigned char)right[i];

        return (left_value > right_value) - (left_value < right_value);
    }
}

int main(void)
{
    printf("%d\n", my_strcmp("apple", "banana"));
    printf("%d\n", my_strcmp("same", "same"));
    printf("%d\n", my_strcmp("zebra", "apple"));
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o my_strcmp
./my_strcmp
```
## 6. 코드 해석
첫 pair는 첫 character에서 작은 값이라 `-1`, 둘째는 terminator까지 같아 `0`, 셋째는 첫 character가 커서 `1`을 출력한다.
## 7. 내부 동작
**[Algorithm]** 공통 prefix length를 `p`라 하면 비교는 `O(p + 1)`, worst case는 shorter string length에 비례한다. auxiliary space는 `O(1)`이다.

**[C17]** 두 input은 valid C string이어야 한다. `unsigned char`로 변환해 byte-string comparison ordering을 표현한다. null pointer나 missing terminator는 contract 위반이다.

**[Compiler / CPU]** comparison result와 generated branch·instruction 수를 동일시하지 않는다.
## 8. 자주 하는 실수
- 첫 차이 뒤에도 계속 비교한다.
- prefix가 같은 `"a"`와 `"aa"`의 terminator를 무시한다.
- arbitrary signed integer comparator에 `a - b`를 사용한다.
- return value가 언제나 정확히 문자 차이라고 일반화한다.
## 9. 필수 실습
less, equal, greater, empty, prefix 관계를 deterministic cases로 검증한다.
[29-9 exercise](../../exercises/29-basic-algorithms/29-9/README.md)
## 10. 추가 실습
- ★ `""`와 `""`를 비교한다.
- ★★ `"a"`와 `"aa"`의 첫 차이를 기록한다.
- ★★★ standard `strcmp`와 sign만 임시 validation에서 비교한다.
## 11. 확인 문제
1. loop가 멈추는 두 조건은?
2. unsigned character로 비교하는 이유는?
3. prefix 관계에서 terminator가 중요한 이유는?
4. `(a > b) - (a < b)`의 결과 범위는?
5. standard `strcmp`에서 보장되는 것은 exact magnitude인가 sign인가?
## 12. 핵심 정리
- 첫 차이 또는 terminator까지만 검사한다.
- comparison contract는 sign을 중심으로 정의한다.
- subtraction overflow 없는 relational comparison을 사용한다.
## 13. 다음 Step
[29-10. GCD](29-10-gcd.md)
## 14. 참고 자료
- N1570 7.24.4.2. N1570은 **C11 공개 Committee Draft**이며 `strcmp` 관련 library contract는 C17에서도 유지된다.
- [cppreference: strcmp](https://en.cppreference.com/w/c/string/byte/strcmp)
