# 13-8. `my_strcpy`

`my_strcpy`는 source의 characters와 마지막 null terminator까지 destination array에 복사한다.

## 1. 학습 목표
- source 내용과 terminator를 모두 복사한다.
- destination capacity가 충분해야 하는 contract를 설명한다.
- array bounds 안에서만 assignment한다.

## 2. 선수 지식
Step 13-3의 terminator, Step 13-6의 array parameter, Part 11의 element assignment를 사용한다.

## 3. 핵심 개념
복사는 source의 `'\0'` 전까지 characters를 옮긴 뒤 destination에도 `'\0'`을 저장해야 끝난 string이 된다. source length + 1 이상의 destination elements가 필요하다. 이 단순 함수는 capacity argument가 없으므로 caller가 충분한 공간을 보장한다.

## 4. 문법
```c
void my_strcpy(char destination[], char source[])
{
    size_t i = 0;
    while (source[i] != '\0') {
        destination[i] = source[i];
        ++i;
    }
    destination[i] = '\0';
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

void my_strcpy(char destination[], char source[])
{
    size_t i = 0;
    while (source[i] != '\0') {
        destination[i] = source[i];
        ++i;
    }
    destination[i] = '\0';
}

int main(void)
{
    char source[] = "Cat";
    char destination[4] = {0};

    my_strcpy(destination, source);
    printf("%s\n", destination);
    return 0;
}
```

## 6. 코드 해석
indices 0~2의 C, a, t를 복사하고 index 3에 null character를 저장한다. destination capacity 4가 source 내용 3 + terminator 1에 정확히 맞아 valid string `"Cat"`이 된다.

## 7. 내부 동작
[C17 표준] destination 범위 밖 write는 Undefined Behavior다. source와 destination이 부적절하게 겹치는 경우도 이 단순 copy contract에 포함하지 않는다. standard `strcpy`의 prototype과 pointer 세부는 후속 Part에서 더 정확히 읽는다.

## 8. 자주 하는 실수
- null terminator를 복사하지 않는다.
- destination을 source 내용 길이만큼만 선언한다.
- destination capacity를 확인하지 않는다.
- overflow를 직접 실행해 결과를 관찰한다.

## 9. 필수 실습
충분한 destination에 `"hello"`를 복사하고 source와 destination을 출력한다. [실습 README](../../exercises/13-characters-and-strings/13-8/README.md)

## 10. 추가 실습
- ★ empty string 복사
- ★★ 한 character string 복사
- ★★★ 필요한 destination capacity 계산 문제

## 11. 확인 문제
1. terminator도 복사해야 하는 이유는?
2. `"Cat"`에 필요한 최소 capacity는?
3. destination이 작으면 어떤 C17 문제가 생기는가?
4. 함수가 caller에게 요구하는 contract는?
5. source와 destination overlap을 허용한다고 가정해도 되는가?

## 12. 핵심 정리
string copy는 내용과 terminator를 모두 옮기며 caller는 destination에 length + 1 이상의 공간을 보장해야 한다.

## 13. 다음 Step
[Step 13-9. `my_strcmp`](13-9-my-strcmp.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 7.24.2.3
- [cppreference: strcpy](https://en.cppreference.com/w/c/string/byte/strcpy.html)
