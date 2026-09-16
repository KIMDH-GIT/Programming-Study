# 13-10. `strlen`과 `sizeof`

`sizeof`는 array object의 전체 C byte 수를, `strlen`은 첫 null character 전까지의 string length를 구한다.

## 1. 학습 목표
- `sizeof(text)`와 `strlen(text)` 결과를 비교한다.
- `strlen`의 return type과 terminator 제외를 설명한다.
- multibyte encoding에서 byte count와 사람의 글자 수를 구분한다.

## 2. 선수 지식
Part 11의 `sizeof(array)`, Step 13-3의 terminator, Step 13-7의 직접 length 계산을 안다.

## 3. 핵심 개념
`char text[] = "hello";`는 six char elements이므로 `sizeof(text)`는 6이다. `strlen(text)`는 null character 앞의 five characters를 세어 5다. 둘 다 `size_t`를 반환하지만 의미가 다르다. array parameter 문맥의 `sizeof`는 원본 array 크기를 주지 않는다.

## 4. 문법
```c
#include <string.h>
size_t bytes = sizeof(text);
size_t length = strlen(text);
```

## 5. 최소 코드 예제
```c
#include <stdio.h>
#include <string.h>

int main(void)
{
    char text[] = "hello";

    printf("sizeof: %zu\n", sizeof(text));
    printf("strlen: %zu\n", strlen(text));
    printf("literal sizeof: %zu\n", sizeof("Cat"));
    return 0;
}
```

## 6. 코드 해석
`text`에는 h,e,l,l,o,`'\0'` 여섯 elements가 있어 `sizeof`는 6이고 `strlen`은 5다. `"Cat"` literal도 terminator를 포함해 `sizeof`가 4다.

## 7. 내부 동작
[C17 표준] `strlen`은 null character 앞의 characters 수를 반환하고 type은 `size_t`다. valid string이 아니면 사용할 수 없다. [encoding 구현] UTF-8 환경에서 사람이 보는 한 글자가 여러 char bytes일 수 있어 `strlen("가")`가 항상 1이라는 보장은 없다.

## 8. 자주 하는 실수
- `sizeof`와 `strlen`을 같은 연산이라고 생각한다.
- `strlen`에 terminator가 포함된다고 생각한다.
- parameter의 `sizeof`로 caller array 크기를 구한다.
- `strlen` 결과를 Unicode 문자 개수라고 일반화한다.

## 9. 필수 실습
empty, one-letter, `"hello"` arrays의 `sizeof`와 `strlen`을 비교한다. [실습 README](../../exercises/13-characters-and-strings/13-10/README.md)

## 10. 추가 실습
- ★ `"Cat"` 비교
- ★★ 중간 null을 가진 array 비교
- ★★★ byte count와 표시 글자 수 차이 조사

## 11. 확인 문제
1. `"hello"` array의 `sizeof`는?
2. `strlen`은?
3. 두 결과형은?
4. terminator는 어느 결과에 포함되는가?
5. UTF-8 한 글자의 `strlen`이 항상 1인가?

## 12. 핵심 정리
`sizeof`는 array storage 전체를, `strlen`은 valid string의 null 전 byte characters를 세며 terminator 포함 여부가 다르다.

## 13. 다음 Step
[Step 13-11. `strcpy`와 목적지 용량](13-11-strcpy-capacity.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.3.4, 7.24.6.3
- [cppreference: strlen](https://en.cppreference.com/w/c/string/byte/strlen.html)
- [cppreference: sizeof](https://en.cppreference.com/w/c/language/sizeof.html)
