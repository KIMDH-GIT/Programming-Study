# 13-11. `strcpy`와 목적지 용량

`strcpy`는 source string의 characters와 null terminator를 destination에 복사하므로 destination capacity가 충분해야 한다.

## 1. 학습 목표
- `strcpy`의 source·destination 역할을 구분한다.
- 필요한 destination capacity를 계산한다.
- capacity 부족을 Undefined Behavior 위험으로 분류한다.

## 2. 선수 지식
Step 13-8의 `my_strcpy`와 Step 13-10의 `strlen`을 안다.

## 3. 핵심 개념
source length가 N이면 destination에는 최소 N+1 char elements가 필요하다. `strcpy` 자체는 destination capacity를 인자로 받지 않으므로 caller가 공간과 overlap contract를 보장해야 한다. 단순히 함수가 존재한다고 항상 안전한 것은 아니다.

## 4. 문법
```c
#include <string.h>
strcpy(destination, source);
```

## 5. 최소 코드 예제
```c
#include <stdio.h>
#include <string.h>

int main(void)
{
    char source[] = "hello";
    char destination[6] = {0};

    strcpy(destination, source);
    printf("%s\n", destination);
    return 0;
}
```

## 6. 코드 해석
source는 letters 5개와 terminator 하나다. destination capacity 6이 정확히 충분하므로 모두 복사되고 `hello`가 안전하게 출력된다.

## 7. 내부 동작
[C17 표준] `strcpy`는 source가 가리키는 string을 terminator까지 destination array로 복사한다. destination이 충분히 크지 않거나 objects가 부적절하게 overlap하면 behavior가 정의되지 않는다. 함수 prototype의 pointer 문법은 Part 14 이후 자세히 다룬다.

## 8. 자주 하는 실수
- destination을 `strlen(source)` 크기로만 만든다.
- `strcpy`가 capacity를 자동 검사한다고 생각한다.
- null 없는 char array를 source로 사용한다.
- overflow 예제를 실행해 결과를 관찰한다.

## 9. 필수 실습
known literal로 초기화된 source를 정확히 충분한 destination에 복사한다. [실습 README](../../exercises/13-characters-and-strings/13-11/README.md)

## 10. 추가 실습
- ★ empty source 복사
- ★★ 더 큰 destination 사용
- ★★★ 여러 source lengths의 최소 capacity 계산

## 11. 확인 문제
1. `"hello"`의 최소 destination capacity는?
2. terminator도 복사되는가?
3. `strcpy`가 capacity를 받는가?
4. destination이 작으면 어떤 문제인가?
5. overlap을 무조건 허용하는가?

## 12. 핵심 정리
`strcpy` caller는 source length + 1 이상의 destination과 valid non-overlapping strings를 보장해야 한다.

## 13. 다음 Step
[Step 13-12. `strcmp`의 반환값](13-12-strcmp-result.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 7.24.2.3
- [cppreference: strcpy](https://en.cppreference.com/w/c/string/byte/strcpy.html)
