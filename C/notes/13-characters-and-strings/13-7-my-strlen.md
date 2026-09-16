# 13-7. `my_strlen`

`my_strlen`은 valid string의 첫 `'\0'` 전까지 char elements를 세어 내용 길이를 반환한다.

## 1. 학습 목표
- terminator를 만날 때까지 안전하게 순회한다.
- 길이에서 terminator를 제외한다.
- 반환형으로 `size_t`를 사용한다.

## 2. 선수 지식
Step 13-3의 null termination과 Step 13-6의 array parameter notation을 사용한다.

## 3. 핵심 개념
index 0에서 시작해 `text[length] != '\0'`인 동안 length를 증가시킨다. 함수는 입력이 접근 가능한 범위 안에서 null-terminated라는 contract를 전제로 한다. 이는 `strlen`의 개념을 학습하는 구현이며 실제 library가 반드시 같은 source loop를 사용한다는 뜻은 아니다.

## 4. 문법
```c
size_t my_strlen(char text[])
{
    size_t length = 0;
    while (text[length] != '\0') {
        ++length;
    }
    return length;
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

size_t my_strlen(char text[])
{
    size_t length = 0;
    while (text[length] != '\0') {
        ++length;
    }
    return length;
}

int main(void)
{
    char text[] = "hello";
    printf("%zu\n", my_strlen(text));
    return 0;
}
```

## 6. 코드 해석
index 0~4의 five letters를 세고 index 5의 null character에서 멈춘다. terminator는 length에 포함되지 않아 5를 반환한다.

## 7. 내부 동작
[C17 표준] `size_t`는 object 크기와 개수를 나타내는 unsigned integer type이다. 함수는 valid string을 전제로 하며 terminator가 없다면 범위 밖을 읽을 수 있다. parameter adjustment의 정확한 pointer 의미는 후속 Part로 미룬다.

## 8. 자주 하는 실수
- terminator까지 1을 더해 반환한다.
- length를 초기화하지 않는다.
- null 없는 char array를 전달해 동작을 관찰한다.
- `sizeof(text)`를 function 안에서 string length로 사용한다.

## 9. 필수 실습
`my_strlen`으로 empty string, `"A"`, `"hello"` 길이를 확인한다. [실습 README](../../exercises/13-characters-and-strings/13-7/README.md)

## 10. 추가 실습
- ★ 빈 string
- ★★ spaces를 포함한 initialized string
- ★★★ index별 character와 count 추적표

## 11. 확인 문제
1. loop는 어떤 값에서 멈추는가?
2. terminator가 length에 포함되는가?
3. 반환형은?
4. 함수가 요구하는 input contract는?
5. parameter에서 `sizeof(text)`가 길이가 아닌 이유는?

## 12. 핵심 정리
`my_strlen`은 valid string의 null character 전까지를 `size_t`로 세며 terminator 자체는 제외한다.

## 13. 다음 Step
[Step 13-8. `my_strcpy`](13-8-my-strcpy.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 7.19, 7.24.6.3
- [cppreference: strlen](https://en.cppreference.com/w/c/string/byte/strlen.html)
