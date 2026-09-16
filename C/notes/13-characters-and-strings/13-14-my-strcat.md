# 13-14. 선택 실습: `my_strcat`

`my_strcat`은 destination terminator를 찾은 뒤 그 위치부터 source와 source terminator를 복사한다.

## 1. 학습 목표
- destination의 현재 끝을 찾는다.
- source를 terminator까지 이어 붙인다.
- caller의 capacity contract를 유지한다.

## 2. 선수 지식
Step 13-8의 `my_strcpy`, Step 13-13의 concatenation capacity를 사용한다.

## 3. 핵심 개념
첫 loop는 destination의 `'\0'` index를 찾는다. 둘째 loop는 source characters를 그 index부터 복사하고 마지막에 새 terminator를 저장한다. destination에는 기존 length + source length + 1 이상의 elements가 있어야 한다.

## 4. 문법
```c
void my_strcat(char destination[], char source[]);
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

void my_strcat(char destination[], char source[])
{
    size_t end = 0;
    while (destination[end] != '\0') {
        ++end;
    }

    size_t i = 0;
    while (source[i] != '\0') {
        destination[end + i] = source[i];
        ++i;
    }
    destination[end + i] = '\0';
}

int main(void)
{
    char destination[11] = "C";
    char source[] = " language";

    my_strcat(destination, source);
    printf("%s\n", destination);
    return 0;
}
```

## 6. 코드 해석
destination capacity는 C 1 + source 9 + terminator 1로 11이다. `end`는 index 1을 찾고 source를 index 1부터 복사해 `"C language"`를 만든다.

## 7. 내부 동작
[C17 표준] destination 범위 안 assignment만 정의된다. 함수 자체는 capacity를 알 수 없으므로 caller contract가 깨지면 Undefined Behavior가 될 수 있다.

## 8. 자주 하는 실수
- destination 끝을 찾지 않고 index 0부터 덮어쓴다.
- source terminator를 결과에 저장하지 않는다.
- capacity 계산에서 terminator를 빼먹는다.
- overlap을 지원한다고 가정한다.

## 9. 필수 실습
충분한 destination에 두 short strings를 `my_strcat`으로 연결한다. [실습 README](../../exercises/13-characters-and-strings/13-14/README.md)

## 10. 추가 실습
- ★ empty source
- ★★ empty destination
- ★★★ end와 source index 추적표 작성

## 11. 확인 문제
1. 첫 loop가 찾는 것은?
2. source는 destination의 어느 index부터 복사되는가?
3. result terminator는 어디에 저장되는가?
4. 최소 capacity 식은?
5. 함수가 capacity를 직접 아는가?

## 12. 핵심 정리
`my_strcat`은 destination 끝을 찾고 source와 terminator를 복사하며 충분한 capacity는 caller가 보장한다.

## 13. 다음 Step
[Step 13-15. 문자·대문자·소문자·숫자·공백 개수 분석](13-15-character-count-analysis.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 7.24.3.1
- [cppreference: strcat](https://en.cppreference.com/w/c/string/byte/strcat.html)
