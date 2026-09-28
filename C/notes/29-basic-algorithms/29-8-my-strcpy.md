# 29-8. `my_strcpy`
## 1. 학습 목표
- source string과 destination capacity contract를 정의한다.
- terminating null character까지 복사한다.
- destination 부족과 overlap을 실행 전에 배제한다.
## 2. 선수 지식
29-7의 string precondition과 Part 17의 `const`를 안다.
## 3. 핵심 개념
**[Algorithm]** source의 non-null character와 마지막 `'\0'`을 destination에 같은 순서로 복사한다.

이 교육용 함수는 destination capacity를 받아 실패를 표현한다. source와 destination이 overlap하지 않는다는 조건도 필요하다. buffer가 부족하면 부분 복사를 만들지 않고 0을 반환한다.
## 4. 문법
```c
static int my_strcpy(char destination[], size_t capacity,
                     const char source[]);
```

입력 조건:
- `source`는 valid C string이다.
- `destination`은 `capacity` bytes를 나타낸다.
- 두 object range는 overlap하지 않는다.

결과 조건:
- 성공하면 destination은 source와 같은 C string이고 1을 반환한다.
- capacity가 부족하면 destination을 수정하지 않고 0을 반환한다.
## 5. 최소 코드 예제
```c
#include <stddef.h>
#include <stdio.h>

static int my_strcpy(char destination[], size_t capacity,
                     const char source[])
{
    size_t length = 0;

    if (destination == NULL || source == NULL || capacity == 0) {
        return 0;
    }
    while (source[length] != '\0') {
        ++length;
    }
    if (length > capacity - 1) {
        return 0;
    }
    for (size_t i = 0; i <= length; ++i) {
        destination[i] = source[i];
    }
    return 1;
}

int main(void)
{
    char enough[4];
    char small[3] = "";

    printf("copied=%d\n", my_strcpy(enough, sizeof enough, "C17"));
    printf("text=%s\n", enough);
    printf("small=%d\n", my_strcpy(small, sizeof small, "C17"));
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o my_strcpy
./my_strcpy
```
## 6. 코드 해석
capacity 4에는 세 character와 terminator가 들어간다. capacity 3은 부족하므로 실패하고 부분 string을 만들지 않는다. 출력은 `copied=1`, `text=C17`, `small=0`이다.
## 7. 내부 동작
**[Algorithm]** source 길이를 확인한 뒤 terminator를 포함해 복사한다. source length를 `n`이라 하면 두 pass 모두 합쳐 `O(n)`, auxiliary space는 `O(1)`이다.

**[C17]** `capacity == 0`을 먼저 거부해 `capacity - 1` underflow를 피한다. `i <= length`는 validated capacity 아래에서 terminator까지 복사한다. overlap은 이 function contract 밖이다.

**[Compiler / CPU]** library `strcpy` 구현을 복사한 것이 아니며 compiler builtin 최적화와도 별개다.
## 8. 자주 하는 실수
- terminator 공간을 빼먹는다.
- capacity 0에서 `capacity - 1`을 계산한다.
- 실패 전에 destination 일부를 덮어쓴다.
- overlap input을 허용하면서 이동 방향을 검토하지 않는다.
## 9. 필수 실습
exact fit, larger buffer, one-byte-short, empty source를 검증한다.
[29-8 exercise](../../exercises/29-basic-algorithms/29-8/README.md)
## 10. 추가 실습
- ★ empty source를 capacity 1에 복사한다.
- ★★ 실패 시 destination unchanged를 검사한다.
- ★★★ one-pass 설계와 partial-write contract의 trade-off를 비교한다.
## 11. 확인 문제
1. destination에 필요한 최소 capacity는?
2. terminator도 복사해야 하는 이유는?
3. `capacity == 0`을 먼저 검사하는 이유는?
4. overlap을 별도 precondition으로 두는 이유는?
5. 실패 시 partial copy를 하지 않는 장점은?
## 12. 핵심 정리
- source validity, destination capacity, overlap을 contract에 포함한다.
- terminator까지 복사해야 destination이 C string이 된다.
- capacity 부족은 bounds access 전에 판정한다.
## 13. 다음 Step
[29-9. `my_strcmp`](29-9-my-strcmp.md)
## 14. 참고 자료
- N1570 7.24.2.3. N1570은 **C11 공개 Committee Draft**이며 `strcpy` 관련 library contract는 C17에서도 유지된다.
- [SEI CERT C: STR31-C](https://wiki.sei.cmu.edu/confluence/display/c/STR31-C.+Guarantee+that+storage+for+strings+has+sufficient+space+for+character+data+and+the+null+terminator)
